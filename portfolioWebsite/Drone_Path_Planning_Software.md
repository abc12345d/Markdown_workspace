# Drone Path Planning Software
A medium-scale Java software that plans a drone's flying path with certain constraints and produces visualisations of the flight path and various sensor stations using GeoJSON.

---

According to the program specifications, the drone has to visit 33 sensors which are listed on the file named air-quality-data.json and return close to the starting point of the flight path. Besides, the drone is a battery-powered device so the number of moves it can make is limited to 150 moves. Hence, we could not let the drone simply fly around and expect it can finish the requirements within 150 moves. In order to satisfy the requirements, there are two important question we have to solve:
- How to decide the order to visit the 33 specific sensors?
- How their flight around the air-quality sensors and back to the start location of their flight, while avoiding all of the no-fly-zones and remains strictly inside the confinement area?

## Deciding the order
Since we have to visit all the 33 sensors within 150 moves, the shorter the distance between the sensors, the higher the probability the full flight path is short. Hence, I implemented the Greedy algorithm to address this problem.

The greedy algorithm builds a tour by always moving to the closest sensor (among the sensors not yet been visited) from the starting point which guarantees the distance between every two sensors in the tour is the shortest. There is a flowchart of the Greedy algorithm inside my report.

Although the greedy algorithm makes the optimal choice at each step when it attempts to build the tour, it may still fail to find the shortest tour since it do not consider all the sensors and unaware of future choices it could make. Thus, I think a tour improvement algorithm is required for improving the tour obtained from greedy algorithm. I chose 2-Opt algorithm as the tour improvement algorithm.

The 2-Opt algorithm considers the cost (in terms of distance) of a contiguous sequence of sensors on the current tour and then considers the effect on cost of these sensors be visited in the reverse order. If the reversal will make the cost of the tour lesser, then change the corresponding part of tour to this reversal. 2-Opt algorithm improve the initial tour gradually until there is no more improvement can be made.

In summary, I use greedy algorithm to construct a initial order to visit the 33 sensors and I then use 2-Opt algorithm to improve the initial tour.

<img width="639" alt="comparison of algorithms" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/3f377929-7408-4efb-bacf-5d2105f7e0e3">

## Avoiding obstacles and remaining in confinement area
First of all, we have to define what is an obstacle means in the program. I implemented a single obstacle as a list of *line2D.Double* and a move is considered illegal if the straight line of length 0.0003 degrees intersects with any lines in the list. Due to that, I can now implement the confinement area as 4 *line2D.Double* objects and all the buildings included the confinement area form a big list of *line2D.Double* named no-fly-zone. Before making any move, the program would calculate the degree of next possible move:

> degree = arctan 2(y1 − y2, x1 − x2)

where (x1,y1) is the current point and (x2,y2) is the next sensor point. Since the drone can only fly in a direction which is a multiple of ten degrees1. The program would round the degree to the nearest ten and check whether if a move in the degree is illegal. If the move is illegal, then the program would start finding the next valid move by adding 10 degrees every two counter and reverse the sign of number every two counter. The program would loop until it found the next valid move.

<img width="558" alt="strategy for avoiding obstacles" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/295612d9-4029-451b-b18d-b02164294698">

However, there is a flaw in this mechanism. The drone might stuck in somewhere within the confinement area. It stuck flying back and forth until it reaches the maximum limit number of moves and fail to return to the starting point.

<img width="359" alt="The drone is stuck." src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/956802c0-7f10-480a-9459-779501a49885">

Therefore, random fly might be needed if the drone is stuck. I stored a list of last five movements. If the next move is in the No Fly Zones and is exactly same with one of the last five movements, then the drone would set the next degree to move randomly and check if the move is valid and does not stuck until it found a valid move in order to break out of the cycle.

## UML class diagram
The program consists of 8 top classes and 2 nested classes:

<img width="888" alt="UML_class_diagram" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/b6d2a29b-32d1-4a8c-bd41-a46373261727">

## Class documentation

<img width="888" alt="App class" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/a0afe5df-0c46-4fef-8215-555f5a4a6945">
<img width="892" alt="Drone class and PathFinder Class" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/619d292d-f5bb-40ea-9934-b05baceb1b53">
<img width="891" alt="Movement class" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/a8e643a5-304e-4cc8-872d-e9a32c62f84c">
<img width="885" alt="Screenshot 2023-08-28 at 19 43 11" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/ab4072f8-2bf5-41e7-bbb8-f01841760458">

## Project report
<a href="https://abc12345d.github.io/file_server/ilp_report.pdf">If nothing shows up, click this link</a>
<object data="https://abc12345d.github.io/file_server/ilp_report.pdf" type="application/pdf" width="100%" height="1000vh">
</object>
