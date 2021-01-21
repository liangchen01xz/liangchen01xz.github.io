---
layout: post
title: Questasim波形显示状态机名称
subtitle:
author: Liang Chen
date: 2020-01-18 18:00:00 +0800
tags: [Notes, EDA]
catalog: true
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

> Reference: [https://www.cnblogs.com/xianyufpga/p/11326100.html](https://www.cnblogs.com/xianyufpga/p/11326100.html)

在do文件中加入以下代码

```verilog
virtual type {
  {6'b000000 STATE_RST             }
  {6'b000001 STATE_IDLE            }
  {6'b000010 STATE_CFG_RAM         }
  {6'b000011 STATE_WEIGHT_RAM      }
  {6'b000100 STATE_PROCESS_3       }
  {6'b000101 STATE_PROCESS_3_WAIT  }
  {6'b000110 STATE_LOAD_INST       }                                                                                                                                                       
  {6'b000111 STATE_DECODE_INST     }
  {6'b001000 STATE_PROCESS_0       }
  {6'b001001 STATE_PROCESS_1       }
  {6'b001010 STATE_PROCESS_2       }
  {6'b001011 STATE_WRITE           }
  {6'b001100 STATE_WRITE_DONE      }
  {6'b001101 STATE_PROCESS_4_1     }
  {6'b001110 STATE_BRANCH_DONE     }
  {6'b001111 STATE_UPDATE_INST     }
  {6'b010000 STATE_DATA_END        }
  {6'b010001 STATE_OUTPUT          }
  {6'b010010 STATE_OUTPUT_DONE     }
  {6'b010011 STATE_RL              }
  {6'b010100 STATE_CONV            }
  {6'b010101 STATE_POOL            }
  {6'b010110 STATE_DENSE           }
  {6'b010111 STATE_ERROR           }
  {6'b011000 STATE_BRANCH_LEVEL_1  }
  {6'b011001 STATE_BRANCH_LEVEL_2  }
  {6'b011010 STATE_UPDATE_R_C      }
  {6'b011011 STATE_DELAY           }
  {6'b011100 STATE_BRANCH_CONTINUE }
  {6'b011101 STATE_BETWEEN_P1_P2   }
  {6'b011110 STATE_LOAD_DATA_WAIT  }
  {6'b011111 STATE_LOAD_DATA_PRE   }
  {6'b100000 STATE_WEIGHT_RAM_DELAY}
  {6'b100001 STATE_LSTM            }
  {6'b100010 STATE_LSTM_1          }
  {6'b100011 STATE_LSTM_2          }
  {6'b100100 STATE_LSTM_3          }
  {6'b100101 STATE_LSTM_4          }
  {6'b100110 STATE_LSTM_5          }
  {6'b100111 STATE_PROCESS_4_2     }
  {6'b101000 STATE_PROCESS_4_3     }
  {6'b101001 STATE_PROCESS_4_4     }
} state_type
virtual function {(state_type)/ok_for_nnic_tb/interface_with_design/U0/U0/state_ctrl} state_ctrl_display
add wave -color pink ok_for_nnic_tb/interface_with_design/U0/U0/state_ctrl_display
```
