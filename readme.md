# Recruits Task - Week #2
- ## [git para noobs](https://hackmd.io/@PedroRomao/HJ0GJSae1x)
- ## Add this file to your local git repository create a new branch and work on it, when you are done or as you complete the questions merge the branch into the main branch (remote main branch will have your final work, this means main branch on GitHub, please make the repository public for the next weeks).
- ## Perform the following tasks to the best of your hability, sometimes, in the questions, there are multiple answers just tell us what you think, feel free to use the group to ask questions.

### 1
**1.** Check out the [el-sw repository](https://github.com/fs-feup/el-sw/tree/main) code and documentation  and try to generally understand what the software does in each device (there is no need to understand all the little details).
### 2
When we read values from the brake sensor (C1) and the apps (C3) we do not use the most recent reading and use instead a different approach. Explain the approach and why you think it is used.

**Answer:** When reading the values from brake sensor and apps, the programm doesn't use the most recent reading, instead it performs a "moving average".
The programm picks the last few reads, and averages it. This method is used so that the reads can be more stable with smaller variation. I think this method also reduced eletrical noise and other interference with the sensors. Also this minimizes wrong data being produced due to spikes or drops, so the system is more reliable.
Exemples of code used in 01



### 3
Check out the R2D(Ready To Drive) code on the C3 state machine. In the condition below we use a timer (R2DTimer) to check the brake was engaged instead of just checking the brake pressure received from can, why?
```c++
        if ((r2dButton.fell() and TSOn and R2DTimer < R2D_TIMEOUT) or R2DOverride)
        {
            playR2DSound();
            initBamocarD3();
            request_dataLOG_messages();
            R2DStatus = DRIVING;
            break;
        }

```

**Answer:** According to the if statement above, if we want to switch the RD2status to DRIVING, the TS must on and r2dbutton must be pressed at the same time as the brakes. The value read of the brakes has to be above 165 in order for the brakes count as pressed. fell() tells us when the button is pressed down after being released (changes from LOW to HIGH). Well there is a problem if the driver presses the brake but quickly releases de r2dButton, because the value of brakes may drop below 165 before the car can switch to driving. To fix this problem, we use R2DTimer, when the brake value is above 165, the R2DTimes is reset to 0. Then, the driver has R2D_TIMEOUT miliseconds to release r2dButton without the car turning off.
### 4
What is the ID of the can message sent to the bamocar to request torque?
**Answer:**  Based on the CAN Table available in the Team's Google Drive, there is a row with the comment "Torque Request to Bamocar." Therefore, the ID of the CAN message sent to the Bamocar to request torque is 0x201.  

### 5 
The code below is not amazing, tell us some things you would change to improve it, you can write them down in text or correct the code:
```c++
// This is a class for my car
class MyCar {
private:
    int sensorReadings[8]; // Array to store readings from 8 sensors

public:
    // Constructor to initialize sensor readings to 0
    MyCar() {
        for (int i = 0; i < 8; i++) {
            sensorReadings[i] = 0; // Set initial readings to 0
        }
    }

    // Method to update readings and print them
    void updatePrint() {
        for (int i = 0; i < 8; i++) {
            sensorReadings[i] = analogRead(i); // Read sensor values from pins 0 to 7
        }
        printReadings(); // Call the method to print readings
    }

    // Function to print the readings of the sensors
    void printReadings() {
        for (int i = 0; i < 8; i++) {
            Serial.print("Sensor Reading ");
            Serial.print(i + 1); // Print sensor index (1-based)
            Serial.print(": ");
            Serial.println(sensorReadings[i]); // Print the reading
        }
        // All readings have been printed
    }
};
```