# CPU积分变化示例

突发性能实例创建成功后，其CPU积分根据CPU使用率和基准性能的关系变化。本文介绍在不同的性能模式下的CPU积分变化情况。

## 背景信息

示例为说明基准性能、CPU积分等概念而设计，具体业务场景更加复杂多变，例如CPU使用率不太可能长时间保持在特定的值。请您在理解突发性能实例相关概念的基础上选择合适的实例，并在必要时管理性能模式或变配实例。具体操作，请参见[切换性能模式](/cn.zh-CN/实例/选择实例规格/突发型/切换性能模式.md)或[变配说明](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)。

在开始阅读示例前，您可能需要了解以下信息：

-   突发性能创建成功后，每个vCPU可以获得30个初始CPU积分。
-   CPU积分的消耗速度和突发性能实例的vCPU数、CPU使用率和工作时间有关。1个CPU积分可供1个vCPU以100%的性能运行1分钟，当实际性能为其它值时，运行时间按比例折算。
-   以基准性能运行时，实例获得的CPU积分和消耗的CPU积分相等。

更多信息，请参见[基准性能](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)和[CPU积分](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)。

## 性能约束模式

在性能约束模式下，初始CPU积分和CPU积分余额消耗完毕后，突发性能实例的性能将无法超过基准性能。

下图以ecs.t6-c2m1.large实例（2 vCPU，1 GiB）为例，展示了性能约束模式下的CPU积分变化。请注意：

-   ecs.t6-c2m1.large实例有2个vCPU，可以获得60个初始CPU积分。
-   ecs.t6-c2m1.large实例的基准性能为10%。
-   ecs.t6-c2m1.large实例每小时可获得12个CPU积分，最大CPU积分余额为288。更多规格信息请参见[t6实例规格表](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)。
-   ecs.t6-c2m1.large实例有2个vCPU，以基准性能运行时每小时消耗12个CPU积分。

![性能约束模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2244872161/p45896.png)

下面为您解释图中各阶段CPU积分变化的原因。

-   0 h ~ 24 h

    A阶段：实例开机获得60个初始CPU积分。由于CPU使用率为0%，CPU积分余额不断增加，直至在24 h达到上限。

    结束时，可用的CPU积分（348） = 初始CPU积分（60） + 最大CPU积分余额（288）。

-   25 h ~ 48 h
    1.  B阶段：CPU使用率为10%，虽然等于基准性能，但优先消耗初始CPU积分。实例运行时，每小时消耗12个CPU积分，60个初始CPU积分消耗完毕后不会恢复。

        结束时，可用的CPU积分（288） = A阶段结束时可用的CPU积分（348） - 消耗的初始CPU积分（60）。

    2.  C阶段：CPU使用率为5%，虽然低于基准性能，但CPU积分余额已达上限，保持不变。

        结束时，CPU积分余额（288） = 最大CPU积分余额（288）。

    3.  D阶段：CPU使用率为10%，等于基准性能，实例获得和消耗的CPU积分相等，CPU积分余额保持不变。

        结束时，CPU积分余额（288） = 最大CPU积分余额（288）。

-   49 h ~ 72 h
    1.  E阶段：CPU使用率为100%，实例运行2小时，每小时消耗120个CPU积分，基准性能无法满足需求，开始消耗CPU积分余额。

        结束时，CPU积分余额（72） = 最大CPU积分余额（288） - 2 \* 每小时消耗的CPU积分（120） + 2 \* 每小时获得的CPU积分（12）。

    2.  F阶段：CPU使用率为0%，实例闲置4小时，每小时获得12个CPU积分，全部转化为CPU积分余额。

        结束时，CPU积分余额（120） = E阶段结束时的CPU积分余额（72） + 4 \* 每小时获得的CPU积分（12）。

    3.  G阶段：CPU使用率为5%，实例运行8小时，每小时消耗6个CPU积分，剩余部分转化为CPU积分余额。

        结束时，CPU积分余额（168） = F阶段结束时的CPU积分余额（120） - 8 \* 每小时消耗的CPU积分（6） + 8 \* 每小时获得的CPU积分（12）。

    4.  H阶段：CPU使用率为80%，基准性能无法满足需求，实例运行2小时，每小时消耗96个CPU积分，CPU积分余额也消耗完毕。在性能约束模式下，无可用的CPU积分时实例将无法突破基准性能。

        **说明：** CPU积分余额较少时，实例性能将在15分钟内逐渐下降到基准性能水平，保证CPU积分余额消耗完毕后，实例性能不会急剧下降。

        结束时，CPU积分余额（0） = G阶段结束时的CPU积分余额（168） - 2 \* 每小时消耗的CPU积分（96） + 2 \* 每小时获得的CPU积分（12）。

    5.  I阶段：CPU使用率为10%，等于基准性能，实例获得和消耗的CPU积分相等，CPU积分余额保持不变。

        结束时，CPU积分余额（0） = H阶段结束时的CPU积分余额（0） - 5 \* 每小时消耗的CPU积分（12） + 5 \* 每小时获得的CPU积分（12）。

    6.  J阶段：CPU使用率为0%，实例闲置3小时，每小时获得12个CPU积分，全部转化为CPU积分余额。

        结束时，CPU积分余额（36） = I阶段结束时的CPU积分余额（0） + 3 \* 每小时获得的CPU积分（12）。


## 无性能约束模式

在无性能约束模式下，突发性能实例可以突破可用CPU积分的约束，通过透支或付费使用CPU积分在任意时间段保持高于基准性能的CPU使用率。

下图以ecs.t6-c1m1.large实例（2 vCPU，2 GiB）为例，展示了无性能约束模式下的CPU积分变化。请注意：

-   ecs.t6-c1m1.large实例有2个vCPU，可以获得60个初始CPU积分。
-   ecs.t6-c1m1.large实例的基准性能为20%。
-   ecs.t6-c1m1.large实例每小时可获得24个CPU积分，最大CPU积分余额为576。更多规格信息请参见[t6实例规格表](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)。
-   ecs.t6-c1m1.large实例有2个vCPU，以基准性能运行时每小时消耗24个CPU积分。

![无性能约束模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3244872161/p46048.png)

下面为您解释图中各阶段CPU积分变化的原因。

-   0 h ~ 24 h

    A阶段：实例开机获得60个初始CPU积分。由于CPU使用率为0%，CPU积分余额不断增加，直至在24 h达到上限。

    结束时，可用的CPU积分（636） = 初始CPU积分（60） + 最大CPU积分余额（576）。

-   25 h ~ 48 h
    1.  B阶段：CPU使用率为20%，虽然等于基准性能，但优先消耗初始CPU积分。实例运行时，每小时消耗24个CPU积分，60个初始CPU积分消耗完毕后不会恢复。

        结束时，可用的CPU积分（576） = A阶段结束时可用的CPU积分（636） - 消耗的初始CPU积分（60）。

    2.  C阶段：CPU使用率为20%，等于基准性能，实例获得和消耗的CPU积分相等，CPU积分余额保持不变。

        结束时，CPU积分余额（576） = 最大CPU积分余额（576）。

    3.  D阶段：CPU使用率为10%，虽然低于基准性能，但CPU积分余额已达上限，保持不变。

        结束时，CPU积分余额（576） = 最大CPU积分余额（576）。

    4.  E阶段：CPU使用率为100%，实例运行时，每小时消耗120个CPU积分，基准性能无法满足需求，开始消耗CPU积分余额。

        结束时，CPU积分余额消耗完毕。

    5.  F阶段：CPU使用率为100%，实例运行时，每小时消耗120个CPU积分，基准性能无法满足需求，开始消耗预支CPU积分。更多说明，请参见[无性能约束模式](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)。

        结束时，预支CPU积分消耗完毕，共透支576个CPU积分。

    6.  G阶段：CPU使用率为100%，实例运行时，每小时消耗120个CPU积分，基准性能无法满足需求，开始付费使用超额CPU积分。更多说明，请参见[无性能约束模式](/cn.zh-CN/实例/选择实例规格/突发型/突发性能实例概述.md)。

        结束时，可用的CPU积分不变，透支576个CPU积分。

-   49 h ~ 72 h

    H阶段：CPU使用率为0%，获得的CPU积分优先恢复预支CPU积分，直至在72 h恢复完毕。

    结束时，无透支的CPU积分，但CPU积分余额仍然为0。

-   73 h ~ 96 h

    H阶段：CPU使用率为0%，实例闲置24小时，每小时获得24个CPU积分，全部转化为CPU积分余额，直至在96 h达到上限。

    结束时，CPU积分余额（576） = 最大CPU积分余额（576）。


