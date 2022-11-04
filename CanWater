/*
 * подключение Arduino Nano к Can шине HW-184 
 * VCC - 5 V
 * GRD - GRD
 * CS - D10
 * SO - D12
 * SI - D11
 * SCK - D13
 * INT - D2
 */

#include <SPI.h>       
#include <DMD.h>    
#include <TimerOne.h>  

#define DISPLAYS_ACROSS 3 //-> Number of P10 panels used, side to side.
#define DISPLAYS_DOWN 1
DMD dmd(DISPLAYS_ACROSS, DISPLAYS_DOWN);
 
#include <SPI.h>
#include <mcp2515.h>

struct can_frame canMsg;

MCP2515 mcp2515(10);

void ScanDMD() { 
  dmd.scanDisplayBySPI();
}

void setup() {

  Serial.begin(115200);
  
  mcp2515.reset();
  mcp2515.setBitrate(CAN_500KBPS);
  mcp2515.setNormalMode();
  
  //Serial.println("------- CAN Read ----------");
  //Serial.println("ID  DLC   DATA");

  Timer1.initialize(1000);          
  Timer1.attachInterrupt(ScanDMD);   
  dmd.clearScreen(true);  
}

void loop() {

  
  if (mcp2515.readMessage(&canMsg) == MCP2515::ERROR_OK) {
    //Serial.print(canMsg.can_id, HEX); // print ID
    //Serial.print(" "); 
    //Serial.print(canMsg.can_dlc, HEX); // print DLC
    //Serial.print(" ");
    
    for (int i = 0; i<canMsg.can_dlc; i++)  {  // print the data
      //Serial.print(canMsg.data[7],DEC);
      //Serial.print(canMsg.data[0]);
      
    }   
    
  }
  //Serial.println(canMsg.data[7]);
    
   checkValue (); 
}

void checkValue (){
    dmd.clearScreen( true );
    int WaterReading = canMsg.data[7];
    //int oldval;
    //Serial.println(WaterReading);
    int range = map(WaterReading, 0, 100, -1, 97);
    // if (range != oldval){
   //     //dmd.clearScreen( true );
   //     delay(1000);
    //}
    //oldval = range;
    dmd.drawFilledBox( 0, 0, range, 15, GRAPHICS_NORMAL );
    delay(1000); 
    
}
