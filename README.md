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
  </tr>
  <tr>
    <td>H1</td>
    <td>Static obstacle not detected</td>
    <td>Vehicle collides with obstacle</td>
    <td>Vehicle moving at 0.7–1 m/s</td>
    <td>S1</td>
    <td>E4</td>
    <td>C3</td>
    <td>ASIL B</td>
  </tr>
  <tr>
    <td>H2</td>
    <td>False obstacle detceted </td>
    <td>Unexpected stop</td>
    <td> - </td>
    <td>S1</td>
    <td>E4</td>
    <td>C2</td>
    <td>ASIL A</td>
  </tr>
</table>

