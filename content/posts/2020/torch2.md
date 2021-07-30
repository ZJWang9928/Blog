---
title: "Pytorch部分加载预训练ResNet-50模型作为Backbone"
date: 2020-01-28T00:08:03+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "pytorch", "python"]
draft: true
---

在写CV实(lian)验(dan)代码的时，经常需要使用预训练好的CNN模型作为自己模型的骨架网络(Backbone)，其中[VGG-16](https://godlovesjonny.github.io/2020/torch1/)和[ResNet-50](https://godlovesjonny.github.io/2020/torch2/)是较为常用的两种Backbone。   
    
    
在使用Backbone时通常会丢弃最后一层全连接，对于具体的任务有时还会有其他的一些修改，因此只需要加载部分预训练模型。   
    

这里介绍部分加载预训练ResNet-50的Pytorch实现方法。   

### 定义需要使用的部分ResNet-50结构
    
    def Conv1p(in_planes, places, stride=2):
        return nn.Sequential(
            nn.Conv2d(in_channels=in_planes, out_channels=places, kernel_size=7, stride=stride, padding=3, bias=False),
            nn.BatchNorm2d(places),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=3, stride=2, padding=1)
        )

    def Conv1(in_planes, places, stride=2):
        return nn.Sequential(
            nn.Conv2d(in_channels=in_planes, out_channels=places, kernel_size=7, stride=stride, padding=3, bias=False),
            nn.BatchNorm2d(places),
            nn.ReLU(inplace=True),
    #         nn.MaxPool2d(kernel_size=3, stride=2, padding=1)
        )

    class Bottleneck(nn.Module):
        def __init__(self,in_places,places, stride=1, downsampling=False, expansion=4):
            super(Bottleneck,self).__init__()
            self.expansion = expansion
            self.downsampling = downsampling

            self.bottleneck = nn.Sequential(
                nn.Conv2d(in_channels=in_places, out_channels=places, kernel_size=1, stride=1, bias=False),
                nn.BatchNorm2d(places),
                nn.ReLU(inplace=True),
                nn.Conv2d(in_channels=places, out_channels=places, kernel_size=3, stride=stride, padding=1, bias=False),
                nn.BatchNorm2d(places),
                nn.ReLU(inplace=True),
                nn.Conv2d(in_channels=places, out_channels=places*self.expansion, kernel_size=1, stride=1, bias=False),
                nn.BatchNorm2d(places*self.expansion),
            )

            if self.downsampling:
                self.downsample = nn.Sequential(
                    nn.Conv2d(in_channels=in_places, out_channels=places*self.expansion, kernel_size=1, stride=stride, bias=False),
                    nn.BatchNorm2d(places*self.expansion)
                )
            self.relu = nn.ReLU(inplace=True)
        def forward(self, x):
            residual = x
            out = self.bottleneck(x)

            if self.downsampling:
                residual = self.downsample(x)

            out += residual
            out = self.relu(out)
            return out

    class ResNet(nn.Module):
        def __init__(self, blocks, num_classes=1000, expansion=4):
            super(ResNet, self).__init__()
            self.expansion = expansion

            self.conv1 = Conv1(in_planes=3, places=64)

            self.layer1 = self.make_layer(in_places=64, places=64, block=blocks[0], stride=1)
            self.layer2 = self.make_layer(in_places=256, places=128, block=blocks[1], stride=2)
            self.layer3 = self.make_layer(in_places=512, places=256, block=blocks[2], stride=2)
            self.layer4 = self.make_layer(in_places=1024, places=512, block=blocks[3], stride=2)

            # self.avgpool = nn.AvgPool2d(7, stride=1)
            # self.fc = nn.Linear(2048, num_classes)

            for m in self.modules():
                if isinstance(m, nn.Conv2d):
                    nn.init.kaiming_normal_(m.weight, mode='fan_out', nonlinearity='relu')
                elif isinstance(m, nn.BatchNorm2d):
                    nn.init.constant_(m.weight, 1)
                    nn.init.constant_(m.bias, 0)

        def make_layer(self, in_places, places, block, stride):
            layers = []
            layers.append(Bottleneck(in_places, places,stride, downsampling=True))
            for i in range(1, block):
                layers.append(Bottleneck(places*self.expansion, places))

            return nn.Sequential(*layers)


        def forward(self, x):
            x = self.conv1(x)
            x = self.layer1(x)
            x = self.layer2(x)
            x = self.layer3(x)
            x = self.layer4(x)

            # x = self.avgpool(x)
            # x = x.view(x.size(0), -1)
            # x = self.fc(x)
            return x

    def ResNet50():
        return ResNet([3, 4, 6, 3])
        

### 模型定义部分
    class ExampleModel(nn.Module):
        def __init__(self, load_pretrained_weights=True):
            super(ExampleModel, self).__init__()

            self.base = ResNet50()
            
            # 继续声明其他部分

            # 加载预训练模型参数
            if load_weights:
                mod = torchvision.models.resnet50(pretrained=True)
                self._initialize_weights()
                model_keys = list(self.base.state_dict().keys())
                model_dict_1 = {}
                for k in range(len(model_keys)):
                    dic1 = {model_keys[k]:list(mod.state_dict().values())[k]}
                    model_dict_1.update(dic1)
                self.base.load_state_dict(model_dict_1)
            

        def forward(self, x):
            x = self.base(x)

            # 继续其他部分结构搭建 
            
            return x
            
     
        def _initialize_weights(self):
            for m in self.modules():
                if isinstance(m, nn.Conv2d):
                    nn.init.normal_(m.weight, std=0.01)
                    if m.bias is not None:
                        nn.init.constant_(m.bias, 0)
                elif isinstance(m, nn.BatchNorm2d):
                    nn.init.normal_(m.weight, 1, 0.02)
                    nn.init.constant_(m.bias, 0)
                elif isinstance(m, nn.BatchNorm1d):
                    nn.init.normal_(m.weight, 1, 0.02)
                    nn.init.constant_(m.bias, 0)
                
                    

### 优化器声明部分(若需要使用不同的学习率)
    

    resnet_params = list(map(id, model.base.parameters()))
    other_params = filter(lambda p: id(p) not in resnet_params, model.parameters())
    params = [
        {"params": model.base.parameters(), "lr": 0},
        {"params": other_params, "lr": 1e-3},
    ]
    optimizer = torch.optim.Adam(params)
    


