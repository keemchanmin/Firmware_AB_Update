# OTA A/B Firmware Update

## Memory Partition
  **BOOT**        : ORIGIN = 0x08000000,    LENGTH = 32K  
  
  **Partition_A**	: ORIGIN = 0x08008000,    LENGTH = 48K
  
  **Partition_B**	: ORIGIN = 0x08014000,    LENGTH = 48K	

## How It Works

1. **Bootloader Execution**:
   - 최초 BOOT 영역의 `JumpToApplication()` 실행 후 `boot_flag`에 따라 partition (A 또는 B) 로드

2. **OTA Update Process**:
   - partition (A 또는 B) 의 펌웨어는 CAN 통신을 통해 펌웨어 데이터 수신
   -  `FirmwareUpdateStateMachine() `실행 후 수신된 펌웨어는 비활성화된 파티션(A 또는 B)에 저장
   -  `FW_UPDATE_COMPLETE ` 상태 시 , `boot_flag` 변경 후 `NVIC_SystemReset()` 호출
   -  새로운 펌웨어 실행

## HARDWARE 
STM32F103RB
