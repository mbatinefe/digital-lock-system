# Digital Combination Lock with User ID

## Project Overview

https://github.com/user-attachments/assets/137df6bd-5a7f-4af6-9c2d-347d66299bda

## Features
- **User Identification**: Users can enter a 2-digit ID to identify themselves.
- **Password Entry**: Users can enter a 4-digit password.
- **LED Indicators**: LEDs indicate the progress of password entry.
- **Access Control**: The system grants or denies access based on the entered password.
- **Timer**: The system returns to the initial state after approximately 3 seconds of displaying the access result.

## How to Use
1. **Enter User ID**: Press the `ID` button and enter your 2-digit user ID using the keypad.
2. **Enter Password**: Press the `PASS` button and enter your 4-digit password using the keypad.
3. **Access Result**: The `GRANTED` or `DENIED` LED will light up based on the entered password. The system will reset after approximately 3 seconds.

## Components
- **Encoder**: Encodes the keypad inputs.
- **Mode Selector**: Switches between user ID entry and password entry modes.
- **Shifter**: Handles the shifting of digits for user ID and password entry.
- **ROM**: Stores user passwords.
- **Timer**: Manages the timing for access result display.

## Requirements
- **Clock Frequency**: 100Hz
- **Synchronous Design**: All flip-flops are driven by the same clock, with no clock gating.

## File Structure
- `project.dig`: The top-level circuit design.
- `encoder`: Verilog HDL code for the encoder block inside of project.dig file.
- `shifter.dig`: Circuit design for the shifter block.
- `shifter_pwd.dig`: Circuit design for the password shifter block.
- `timer.dig`: Circuit design for the timer block.
- `mode_selector.dig`: Circuit design for the mode selector block.
- `password.hex`: Hex file containing user passwords.

## Encoder Module

```verilog
module encoder(
  input P0, P1, P2, P3, P4, P5, P6, P7, P8, P9,
  input CLK, RST,
  output reg [3:0] DATA,
  output reg DA);
  
  reg pressed;
  wire button;

  assign button = P0|P1|P2|P3|P4|P5|P6|P7|P8|P9;

  always @(posedge CLK, negedge RST) 
    if(~RST)
    begin
      pressed <= 0;
      DATA <= 4'b0000;
    end
    else
    begin
      if(button)
      begin
        if(~pressed)
        begin
          pressed <= 1'b1;
          DA <= 1'b1;
        end
        else
          DA <= 1'b0;
        if      (P0) DATA <= 4'b0000;
        else if (P1) DATA <= 4'b0001;
        else if (P2) DATA <= 4'b0010;
        else if (P3) DATA <= 4'b0011;
        else if (P4) DATA <= 4'b0100;
        else if (P5) DATA <= 4'b0101;
        else if (P6) DATA <= 4'b0110;
        else if (P7) DATA <= 4'b0111;
        else if (P8) DATA <= 4'b1000;
        else         DATA <= 4'b1001;
      end
      else
      begin
        pressed <= 1'b0;
        DA <= 1'b0;
      end        
    end      
endmodule
```

## Shifter Module
![shifter](https://github.com/user-attachments/assets/32d1e98e-f77e-490f-a8ec-678215df4130)

## Shifter Passwords Module
![shifter_pwd](https://github.com/user-attachments/assets/6cb3f4c1-601e-432a-b6b5-f0aacf3fea76)

## Timer Module
![timer](https://github.com/user-attachments/assets/6398d340-4610-4be1-84c4-c05fd24d7559)

## Mode Selector Module
![mode_selector](https://github.com/user-attachments/assets/9a122461-7108-4006-9422-e0f3db1e76d9)

## Password Hex Structure
- Each line after v2.0 raw are passwords. They are stored as 16-bit BCD numbers.
  v2.0 raw
  1234
  5678
  ffff
  ffff
  ffff
..........

