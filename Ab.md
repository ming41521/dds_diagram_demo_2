```mermaid
graph TB
    subgraph 命令控制层
        CMD[cmd_ctrl<br/>命令控制模块]
    end
    
    subgraph DDS核心处理
        DDS0[DDS_Module_DA0<br/>DDS通道0]
        DDS1[DDS_Module_DA1<br/>DDS通道1]
        
        subgraph 波形查找表ROM
            SIN[pROM_sin<br/>正弦波]
            SQUARE[pROM_square<br/>方波]
            TRI[pROM_triangular<br/>三角波]
            SAW[pROM_sawtooth<br/>锯齿波]
            HALF[pROM_halfsin<br/>半正弦波]
            CUSTOM[custom_wave_rom<br/>自定义波形]
        end
    end
    
    subgraph DAC输出层
        ACM[acm2108_test<br/>DAC驱动模块]
        DA0[DAC0<br/>通道0]
        DA1[DAC1<br/>通道1]
    end
    
    %% 命令控制流
    CMD -->|dds_wave_sel| DDS0
    CMD -->|dds_freq_word| DDS0
    CMD -->|dds_wave_sel| DDS1
    CMD -->|dds_freq_word| DDS1
    CMD -->|wave_data| CUSTOM
    CMD -->|waveen| CUSTOM
    
    %% DDS内部处理流
    DDS0 -->|Rom_Addr| SIN
    DDS0 -->|Rom_Addr| SQUARE
    DDS0 -->|Rom_Addr| TRI
    DDS0 -->|Rom_Addr| SAW
    DDS0 -->|Rom_Addr| HALF
    DDS0 -->|Rom_Addr| CUSTOM
    
    DDS1 -->|Rom_Addr| SIN
    DDS1 -->|Rom_Addr| SQUARE
    DDS1 -->|Rom_Addr| TRI
    DDS1 -->|Rom_Addr| SAW
    DDS1 -->|Rom_Addr| HALF
    DDS1 -->|Rom_Addr| CUSTOM
    
    SIN -->|wave_sin| DDS0
    SQUARE -->|wave_square| DDS0
    TRI -->|wave_triangle| DDS0
    SAW -->|wave_sawtooth| DDS0
    HALF -->|wave_halfsin| DDS0
    CUSTOM -->|wave_custom| DDS0
    
    SIN -->|wave_sin| DDS1
    SQUARE -->|wave_square| DDS1
    TRI -->|wave_triangle| DDS1
    SAW -->|wave_sawtooth| DDS1
    HALF -->|wave_halfsin| DDS1
    CUSTOM -->|wave_custom| DDS1
    
    %% DAC输出流
    DDS0 -->|dds_da0_data| ACM
    DDS1 -->|dds_da1_data| ACM
    ACM -->|DA0_Data| DA0
    ACM -->|DA1_Data| DA1
    
    %% 频率控制流
    DDS0 -->|Fre_acc[31:24]| DDS0
    DDS1 -->|Fre_acc[31:24]| DDS1
    
    %% 样式定义
    classDef command fill:#e1f5fe
    classDef dds fill:#f3e5f5
    classDef rom fill:#e8f5e8
    classDef dac fill:#fff3e0
    classDef io fill:#ffebee
    
    class CMD command
    class DDS0,DDS1 dds
    class SIN,SQUARE,TRI,SAW,HALF,CUSTOM rom
    class ACM dac
    class DA0,DA1 io
```
