# I will show you some picture here #
 > 1
 > > 2
 > >  >3
 > >  >![cat 猫](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3535222276,3943130922&fm=26&gp=0.jpg)
 > >  >![dog 狗](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=178721898,4183522783&fm=26&gp=0.jpg)
 > >  >how can we discriminate them?
 > >  >


```
def genDataList(dataRootPath, trainPercent=0.8):
    classDirs = os.listdir(dataRootPath)
    classDirs = [x for x in classDirs if os.path.isdir(os.path.join(dataRootPath,x))]
    listDirTest = os.path.join(dataRootPath, "test.list")
    listDirTrain = os.path.join(dataRootPath, "train.list")
    # 清空原来的数据
    with open(listDirTest, 'w') as f:
        pass
    with open(listDirTrain, 'w') as f:
        pass
    # 随机划分训练集与测试集
    classLabel = 0              # 记录类别的标签编号
    class_detail = []           # 记录每个类别的描述
    classList = []              # 记录所有的类别名
    num_images = 0              # 统计图片的总数量
    for classDir in classDirs:
        classPath = os.path.join(dataRootPath,classDir)     # 获取类别为classDir的图片所在的目录
        imgPaths = os.listdir(classPath)                    # 获取类别为classDir的所有图片名
        # 从中取trainPercent（默认80%）作为训练集
        imgIndex = list(range(len(imgPaths)))
        random.shuffle(imgIndex)
        imgIndexTrain = imgIndex[:int(len(imgIndex)*trainPercent)]
        imgIndexTest  = imgIndex[int(len(imgIndex)*trainPercent):]
        with open(listDirTest,'a') as f:
            for imgIndex in imgIndexTest:
                imgPath = os.path.join(classPath,imgPaths[imgIndex])
                f.write(imgPath + '\t%d' % classLabel + '\n')
        with open(listDirTrain,'a') as f:
            for imgIndex in imgIndexTrain:
                imgPath = os.path.join(classPath,imgPaths[imgIndex])
                f.write(imgPath + '\t%d' % classLabel + '\n')        

        num_images += len(imgPaths)
        
        classList.append(classDir)
        class_detail_list = {}
        class_detail_list['class_name'] = classDir             #类别名称，如dog
        class_detail_list['class_label'] = classLabel          #类别标签，如0,1
        class_detail_list['class_test_images'] = len(imgIndexTest)       #该类数据的测试集数目
        class_detail_list['class_trainer_images'] = len(imgIndexTrain)   #该类数据的训练集数目
        class_detail.append(class_detail_list)     
        classLabel += 1

    # 说明的json文件信息
    readjson = {}
    readjson['all_class_name'] = classList                      # 所有的标签
    readjson['all_class_sum'] = len(classDirs)                  # 总类别数量
    readjson['all_class_images'] = num_images                   # 总图片数量
    readjson['class_detail'] = class_detail                     # 每种类别的情况
    jsons = json.dumps(readjson, sort_keys=True, indent=4, separators=(',', ': '))
    with open(os.path.join(dataRootPath,"readme.json"),'w') as f:
        f.write(jsons)
    print ('---生成数据列表完成！共有%d个标签' % readjson['all_class_sum'])
    print ('------标签分别是：', classList)
    # 返回标签的数量
    return readjson['all_class_sum']
```

<hr>

*back to the READEME.md*
[README](ProfessionalEnglish/main/README.md)
