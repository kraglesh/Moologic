Looping through fewer than 100 objects is generally not performance-intensive. A `for` loop handles such a small number efficiently, and filtering out offscreen objects further reduces the workload. Even when processing all loaded objects, performance remains manageable, and advanced algorithms are often unnecessary.

## With 100 Objects:
<img width="1126" alt="image" src="https://github.com/user-attachments/assets/42bcfe59-501d-465e-a246-979183b7b92b">

## With 3000 Objects:
<img width="1134" alt="image" src="https://github.com/user-attachments/assets/493d892c-0698-437d-9103-23cf594afbc6">

## Interations that cause 1 millisecond delay
<img width="1261" alt="image" src="https://github.com/user-attachments/assets/08bbf411-128d-4793-ad5f-d3b7420aa3d7">
### Note: 
calling functions inside decrease threashold. eg: calling replace() will slow down each interation, i.e, calling performance.now() each while loop iteration.
