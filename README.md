# Functional-Safety-LiDAR

## Item Definition
### Item Name
LiDAR-based Obstacle Detection System

### Purpose
Detect static obstacles in vehicle path and provide obstacle information to the Decision making module to avaoid collision

### Item Functionality
- System shall detect static obstacles within 1.6m.
- system shall detect static obstacles within provided angle range.
- system shall send obstacle information to Decisiom maker.
- system shall report sensor faults or invalid ranges.

## HARA

<table>
  <tr>
    <th>Hazard ID</th>
    <th>Malfunction</th>
    <th>Hazardous Event</th>
    <th>Situation</th>
    <th>Severity</th>
    <th>Exposure</th>
    <th>Controllability</th>
    <th>ASIL</th>
    <th>Justification</th>
  </tr>
  <tr>
    <td>H1</td>
    <td>Static obstacle not detected</td>
    <td>Vehicle collides with obstacle</td>
    <td>Vehicle moving at 0.7–1 m/s</td>
    <td>S1</td>
    <td>E4</td>
    <td>C1</td>
    <td>QM</td>
    <td>The RC vehicle operated at low speed in a controlled laboratory with supervisor oversight</td>
  </tr>
  <tr>
    <td>H2</td>
    <td>False obstacle detceted </td>
    <td>Unexpected stop</td>
    <td> - </td>
    <td>S0</td>
    <td>E4</td>
    <td>C1</td>
    <td>QM</td>
    <td> - </td>
  </tr>
</table>

### Safety Goals
<table>
  <tr>
    <th>Safety Goal ID</th>
    <th>Description</th>
    <th>ASIL</th>
  </tr>
  <tr>
    <td>SG-01</td>
    <td>Prevent collision caused by failure to detect static obstacles</td>
    <td>QM</td>
  <tr>
    <td>SG-02</td>
    <td>minimize unnecessary stopping caused by false obstacle detection</td>
    <td>QM</td>
  </tr>
</table>
    
### Funcational Safety Requirement
<table>
  <tr>
    <th>Functional safety requirement ID</th>
    <th>Description</th>
    <th> Safety Goal</th>
  </tr>
  <tr>
    <td>FSR-01</td>
    <td>The system shall detect static osbtacles within defined monitoring zone</td>
    <td>SG-01</td>
  <tr> 
    <td>FSR-02</td>
    <td>The system shall update obstacle infromation at least every 100 ms</td>
    <td>SG-01</td>
  <tr>
    <td>FSR-03</td>
    <td> The system shall detect LiDAR communication loss within 100 ms</td>
    <td>SG-01</td>
  <tr>
    <td>FSR-04</td>
    <td>Reduce False obstacle detections</td>
    <td>SG-02</td>
  <tr>
    <td>FSR-05</td>
    <td>The system shall enter safe state upon critical detection failure</td>
    <td>SG-01</td>
  </tr>
</table> 

## Technical Safety Concept
### Architecture
 ```mermaid
   graph LR
    %% Style for topics (rounded rectangles with colored fill and border)
    style ScanTopic stroke:#E67E22,stroke-width:3px,fill:#F39C12,rx:10,ry:10,color:#222
    style ObstacleDetection stroke:#E67E22,stroke-width:3px,fill:#F39C12,rx:10,ry:10,color:#222

    %% Nodes (components)
    style LiDARSensor stroke:#000,stroke-width:2px,fill:#fff,color:#000
    style LiDARFilter stroke:#000,stroke-width:2px,fill:#fff,color:#000
    style DecisionCore stroke:#000,stroke-width:2px,fill:#fff,color:#000


    LiDARSensor["LiDAR Sensor"]
    LiDARFilter["Obstacle Detection Node"]
    DecisionCore["Decision Core"]


    ScanTopic["/scan"]
    ObstacleDetection["/obstacle_detected"]

    LiDARSensor -- "sensor_msgs/msg/LaserScan.msg" --> ScanTopic
    ScanTopic -- "sensor_msgs/msg/LaserScan.msg" --> LiDARFilter
    LiDARFilter --> ObstacleDetection
    ObstacleDetection -- "STD_MSGS/MSG/BOOL" --> DecisionCore
   
```
### Technical Safety Requirement
<table>
  <tr>
    <th>TSR ID</th>
    <th>Description</th>
    <th>TSR link</th>
  </tr>
  <tr>
    <td>TSR-01</td>
    <td>The Obstaccle module shall process LiDAR data from the defined monitoring zone</td>
    <td>FSR-01, FSR-04</td>
  <tr> 
  <tr>
    <td>TSR-02</td>
    <td> The obstacle detection module shall publsih the obstcale status at the rate of 10Hz</td>
    <td>FSR-02</td>
  </tr>
  <tr>
    <td>TSR-03</td>
    <td>The obstacle module shall detect the communication loss of LiDAR msg within 100ms</td>
    <td>FSR-03</td>
  </tr>
  <tr>
    <td>TSR-04</td>
    <td>The safety monitor shall issue a stop command upon the detection of a critical fault</td>
    <td>FSR-05</td>
  </tr>
  <tr>
    <td>TSR-05</td>
    <td>The safety monitor shall monitor the freshness of recieved LiDAR messages</td>
    <td>FSR-02, FSR-03</td>
  </tr>
</table>

### Software Safety Requirements
<table>
  <tr>
    <th>SSR ID</th>
    <th>Requirement</th>
    <th>Derived from</th>
  </tr>
  <tr>
    <td>SSR-01</td>
    <td>The Software shall detect obtsacles within the configured thresold range</td>
    <td>TSR-01</td>
  <tr> 
  <tr>
    <td>SSR-02</td>
    <td>The software shall detect the obstacles within the configured angle ranges</td>
    <td>TSR-01</td>
  <tr>
  <tr>
    <td>SSR-03</td>
    <td>Ths software shall publish the LiDAR messages every 100ms or faster</td>
    <td>TSR-02</td>
  </tr>
  <tr>
    <td>SSR-04</td>
    <td>The sofware shall reject the obtsacles detections outside the configured threshold</td>
    <td>TSR-04</td>
  </tr>
  <tr>
    <td>SSR-05</td>
    <td> The software shall consider the obstacles only with continous continous cloud points </td>
    <td> TSR-04</td>
  </tr>
  <tr>
    <td>SSR-06</td>
    <td>The software shall issue the stop command upon the detcetion of a critical fault</td>
    <td>TSR-05</td>
  </tr>
  <tr>
    <td>SSR-07</td>
    <td> The software shall issue stop command upon detection of obstacle within the threshold range</td>
    <td>TSR-05</td>
  </tr>
   <tr>
     <td>SSR-08</td>
     <td>The software shall log-safety related faults for the diagnostics purpose</td>
     <td>TSR-05</td>
   </tr>
 
  <tr>
    <td>SSR-09</td>
    <td>The software shall detect the stale LiDAR data using timestamps</td>
    <td>TSR-06</td>
  </tr>
  <tr>
    <td>SSR-10</td>
    <td>The software shall detect LiDAR message timeout greater than 100 ms</td>
    <td>TSR-03</td>
  </tr>
  <tr>
    <td>SSR-11</td>
    <td> The software shall generate a diagnostic fault code when LiDAR communication is lost</td>
    <td>TSR-03</td>
  </tr>
</table>

### FMEA [Failure Modes and Effect Analysis]
<table>
  <tr>
    <th>Failure mode</th>
    <th>Cause</th>
    <th>Effect</th>
    <th>Detection</th>
    <th>Mitigation</th>
  </tr>
  <tr>
    <td>Lidar Disconnected</td>
    <td>Cable Unplugged</td>
    <td>No obatcle detection</td>
    <td>Timeout monitor</td>
    <td>Stop vehicle</td>
  </tr>
  <tr>
    <td>Stale point cloud</td>
    <td>Driver freeze</td>
    <td>outdated obstacle status</td>
    <td>Timestamp check</td>
    <td>Stop vehicle</td>
  </tr>
  <tr>
    <td>False obstacle detected</td>
    <td>Sensor noise</td>
    <td>unecessary stop</td>
    <td>consecutive 2D-point cloud </td>
    <td>Ignore obstacle</td>
  </tr>

