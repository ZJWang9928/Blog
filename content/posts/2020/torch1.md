---
title: "Pytorch部分加载预训练VGG-16模型作为Backbone"
date: 2020-01-25T17:26:21+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "pytorch", "python"]
draft: false
---

在写CV实(lian)验(dan)代码的时，经常需要使用预训练好的CNN模型作为自己模型的骨架网络(Backbone)，其中[VGG-16](https://godlovesjonny.github.io/2020/torch1/)和[ResNet-50](https://godlovesjonny.github.io/2020/torch2/)是较为常用的两种Backbone。   
    
    
在使用Backbone时通常会丢弃最后一层全连接，对于具体的任务有时还会有其他的一些修改，因此只需要加载部分预训练模型。   
    

这里介绍部分加载预训练VGG-16的Pytorch实现方法。   
    

### 模型定义部分
    
    
    class ExampleModel(nn.Module):
        def __init__(self, load_pretrained_weights=True):
            super(ExampleModel, self).__init__()

            # VGG-16结构
            self.frontend_feat = [64, 64, 'M', 128, 128, 'M', 256, 256, 256, 'M', 512, 512, 512,'M',512,512,512]
            self.base = make_layers(self.frontend_feat, in_channels=3, batch_norm=False)
            
            # 继续声明其他部分

            # 加载预训练模型参数
            if load_weights:
                mod = torchvision.models.vgg16(pretrained=True)
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
                
                    
    # 构建CNN网络
    def make_layers(cfg, in_channels=3, batch_norm=True, dilation=False):
        if dilation:
            d_rate = 2
        else:
            d_rate = 1
        layers = []
        for v in cfg:
            if v == 'M':
                layers += [nn.MaxPool2d(kernel_size=2, stride=2)]
            else:
                conv2d = nn.Conv2d(in_channels, v, kernel_size=3, padding=d_rate,dilation = d_rate)
                if batch_norm:
                    layers += [conv2d, nn.BatchNorm2d(v), nn.ReLU(inplace=True)]
                else:
                    layers += [conv2d, nn.ReLU(inplace=True)]
                in_channels = v
        return nn.Sequential(*layers)



### 优化器声明部分(若需要使用不同的学习率)
    

    vgg_params = list(map(id, model.base.parameters()))
    other_params = filter(lambda p: id(p) not in vgg_params, model.parameters())
    params = [
        {"params": model.base.parameters(), "lr": 0},
        {"params": other_params, "lr": 1e-3},
    ]
    optimizer = torch.optim.Adam(params)


