# GPU

## 决定显卡性能的参数
> 原文地址
````
作者：于轰轰
链接：https://www.zhihu.com/question/27371880/answer/36374898
````

- **显卡架构**：显示芯片各种处理单元的组成和工作模式
    - 参数相同的情况下, 架构越先进, 效率就越高, 性能也就越强. 
    - 以下讨论都是在架构相同的情况下
- **流处理器**, 即着色单元, Pixel Shader（NVidia推出了CUDA并行计算架构, 让显卡可以用于图形渲染以外的目的. 所以把N卡的流处理器称作CUDA core. AMD也有类似的技术, 叫AMD stream, 相应的, AMD的流处理器也叫Stream Processor（SP））； 流处理器是一款显卡最核心的卖点, 同架构显卡中, 流处理器数量越多, 性能也越强大, 但流处理器数量的提升和性能的提升并不成正比, 这也涉及到架构的流处理器效率问题. 
- **核芯频率**, （注意核芯频率不一定等于流处理器频率）这是各大非公版显卡厂商争相占领的高地, 奢华的PCB layout, 超公版的供电接口, 都是为了更稳定的提升频率以获取更强的性能. 但是普通玩家也不必盲目追求过高的频率, 小幅度的频率提升一般伴随着更高昂的售价, 但在游戏中的性能提升微乎其微. 更多的是体现在测试类软件的评分上. 而且为了追求高频率砸掉自己招牌的厂家也不在少数, 几年前的影驰（绰号花驰现在都甩不掉）, 还有近两年的微星hawk系列, 都是因为超高默频而导致花屏等各种问题. 
- **显存及其位宽**：一般来说是越大越好, 这两个参数决定了显卡在高分辨率和高抗锯齿下的表现. 在游戏运行中, 如果显存爆掉了, 即使核心强大, 纸面上帧数很高, 实际的游戏体验还是会一卡一顿. 相当难受. 
- **ROPs（光栅处理单元）**：游戏里的抗锯齿和光影效果越好,对ROPs的性能要求也就越高,否则就可能导致游戏帧数急剧下降.比如同样是某个游戏的最高特效,8个光栅单元的显卡可能只能跑25帧.而16个光栅单元的显卡则可以稳定在35帧以上. 