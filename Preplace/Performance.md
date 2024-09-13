It might appear that looping through all objects is performance-intensive, but that's not the case. Typically, there are fewer than 100 objects on screen, and processing them with a `for` loop is quite efficient. Additionally, implementing filtering to exclude offscreen objects further reduces the workload. Even if your are looping through all objects that are loaded, still algorithms aren't needed.

## With 100 Objects:
<img width="1126" alt="image" src="https://github.com/user-attachments/assets/42bcfe59-501d-465e-a246-979183b7b92b">


## With 3000 Objects:
<img width="1134" alt="image" src="https://github.com/user-attachments/assets/493d892c-0698-437d-9103-23cf594afbc6">

