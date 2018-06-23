---
layout: post
title: "NSFetchedResultsController与UITableView的使用"
description: "iOS"
date: 2017-07-13 17:50:11.000000000 +09:00
tag: iOS
---

## 最近在写一个项目，因为有收藏的原因，用其他存储方式感觉不合适，决定用CoreData，然后尝试了一下NSFetchedResultsController与UITableView的结合使用


```
CoreData 提供了 NSFetchedResultsController类，也就是FRC，FRC可以管理UITableView或者UICollectionView的数据源。
这个数据源主要指的的本地持久化的数据，也可以用这个数据源配合着网络请求数据一起使用。

```

### 一、初始化核心对象类

```

//获取模型文件路径、加载模型文件
let modelUrl = Bundle.main.url(forResource: modelName, withExtension: "momd")
managedObjectModel = NSManagedObjectModel(contentsOf: modelUrl!)

//获取数据化文件路径、初始化存储协调器
let dbUrl = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).last?.appendingPathComponent("DataBase.sqlite")
persistentStoreCoordinator = NSPersistentStoreCoordinator(managedObjectModel: managedObjectModel!)
do {
    try persistentStoreCoordinator?.addPersistentStore(ofType: NSSQLiteStoreType , configurationName: nil, at: dbUrl, options: nil)
} catch {
    print("初始化持久化存储协调器失败-----\(error.localizedDescription)")
}

//配置上下文 用来与数据库进行打交道  Context的初始化方法9.0之后使用下面的，concurrencyType代表着在哪个线程里，private是自己另开辟一个线程
managedObjectContext = NSManagedObjectContext(concurrencyType: .privateQueueConcurrencyType)
managedObjectContext?.persistentStoreCoordinator = persistentStoreCoordinator

```

### 二、几个用到的工具方法

```

	//MARK: - 绑定请求类
    func bindRequest(className: String, predicate predicateString: String?) -> NSFetchRequest<NSFetchRequestResult>? {
        // 创建抓取数据的请求对象  NSFetchRequest：从持久存储中检索数据 
        let request = NSFetchRequest<NSFetchRequestResult>()
        // 抓取哪个实体
        let entity = NSEntityDescription.entity(forEntityName: className, in: managedObjectContext!)

        request.entity = entity
        
        //请求谓词 就跟Sql语句where后面的相似
        if predicateString != nil {
            let predicate = NSPredicate(format: predicateString!)
            request.predicate = predicate
        }
        return request
    }


    //MARK: - 通过类获取所有的属性名称
    func classAttributes(classModel: AnyObject) -> Array<Any> {
        var array = Array<Any>()
        let className = NSStringFromClass(classModel.classForCoder)
        
        var outCount:UInt32 = 0
        let classM = objc_getClass(className)
    
        let properties = class_copyPropertyList(classM as! AnyClass, &outCount)
        
        for i in 0...(Int(outCount) - 1) {
            let aPro: objc_property_t = properties![i]!
            let proName = String(utf8String: property_getName(aPro))
            array.append(proName!)
        }
        return array
    }

```

### 三、CRUD

```

	/**
     *  增加实体(插入数据)
     *
     *  @param entityName 实体的名称
     *
     *  @return
     */
    func create(model: AnyObject) -> Bool {
    	//给定Entity的描述，生成相应的NSManagedObject对象，并插入到ManagedObjectContext中去
        let entity = NSEntityDescription.insertNewObject(forEntityName: NSStringFromClass(model.classForCoder), into: managedObjectContext!)
        
        for propertName in ClassAttributes(classModel: model) {
        	//给实体属性一一赋值
            entity.setValue((model as AnyObject).value(forKey: String(describing: propertName)), forKey: String(describing: propertName))
        }
        //TODO: 在保存数据库之前，最好判断一下 context.hasChanged()的值 如果为值false 是保存不成功的
        return saveContext()
    }


    /**
     *  删除实体(删除数据)
     *
     *  @param entityName 实体的名称
     *  @param predicateString 查询条件
     *  @return
     */
    func remove(model: AnyObject, predicateString: String) -> Bool {
        do {
        	//MARK: - fetch 进行数据库搜索 返回的是一个数组
            let listArray = try managedObjectContext!.fetch(self.bindRequest(className: NSStringFromClass(model.classForCoder), predicate: predicateString)!) as! [NSManagedObject]
            if listArray.count > 0 {
                for entity in listArray {
                    managedObjectContext?.delete(entity)
                }
            }
            return saveContext()
        } catch {
        	print(error.localizedDescription)
            return false
        }
        
    }


    /**
     *  更新实体
     *
     *  @param entityName 实体的名称
     *  @param predicateString 查询条件
     *  @return 
     */
    
    func modify(model: AnyObject, predicateString: String) -> Bool {
        do {
            let listArray = try managedObjectContext!.fetch(self.bindRequest(className: NSStringFromClass(model.classForCoder), predicate: predicateString)!) as! [NSManagedObject]
            if listArray.count > 0 {
                for entity in listArray {
                    for propertName in ClassAttributes(classModel: model) {
                        (entity as AnyObject).setValue(model.value(forKey: String(describing: propertName)), forKey: String(describing: propertName))
                    }
                }
            }
            return saveContext()
        } catch {
        	print(error.localizedDescription)
            return false
        }
    }


     /**
     *  查询实体
     *
     *  @param entityName 实体的名称
     *  @param predicateString 查询条件
     *  @return 返回这个实体的数组
     */
    func findByModel(model: AnyObject, predicateString: String) -> AnyObject? {
        var resultArray = [AnyObject]()
        do {
            let listArray = try managedObjectContext!.fetch(self.bindRequest(className: NSStringFromClass(model.classForCoder), predicate: predicateString)!) as! [NSManagedObject]
            if listArray.count >= 1 {
                for entity in listArray {
                    resultArray.append(entity)
                }
            }
            return resultArray as AnyObject
        } catch {
            print(error.localizedDescription)
            return nil
        }
    }

```



