# 电控组第二次培训作业

- **姓名：王添一**
- **学号：*20263008222***

## 实现思路

1. 将 PA4, PA5, PA6 分别配为 LED_0,1,2
2. 将主频设为 72Mhz，psc 为 7199，ARR 为 4999，启用 tim3 时钟，这样算出 500ms
3. 利用定时器中断来实现，每过 500ms 定时器触发一次中断，将上一次电平和这一次的电平分别反转，实现灯的流动。

### 核心代码片段

```c
uint16_t cnt = 0;
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef* htim)
{
  if (htim == &htim3) {
    if (cnt==0)
    {
      HAL_GPIO_WritePin(LED_0_GPIO_Port, LED_0_Pin, GPIO_PIN_SET);
      cnt++;
    }
    else
    {
      HAL_GPIO_TogglePin(Led_arr[(cnt-1)%3].Port, Led_arr[(cnt-1)%3].Pin);
      HAL_GPIO_TogglePin(Led_arr[cnt%3].Port, Led_arr[cnt%3].Pin);
      cnt++;
    }
  }
}
```

## 工程与演示

- CubeMX 工程：`Test.ioc`
- 编译与烧录：CLion + OpenOCD + ST-Link
- 演示视频：[demo.mp4](./demo.mp4)
