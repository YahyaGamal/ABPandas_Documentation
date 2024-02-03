# ABPandas Documentation
## Installation
Install using pip as follows
```bash
pip install abpandas
```
You can also download a wheel file from [PYPI](https://pypi.org/project/abpandas/#files). First, install `wheel` and then install the wheel file as follows
```bash
pip install wheel
pip install abpandas-VERSION-py3-none-any.whl
```
Once installed, you can import `ABPandas` into your python script as `abp`
```python
import ABPandas as abp
```

## Classes

### Agent `abp.Agent(properties)`
- `properties`: dictionary, default= {}  
    - the properties of the agent object (must be a dictionary input for the abp.Model methods to function properly)
- `location_index`: int, default= None  
    - the index to which the agent is assigned (accessed by the Model class when creating or moving the Agent)
- `my_class`: str
    - the name of the agent class in the model (must be defined as a string in the `__init__` function of the child class definition)

#### Examples
Child class definition of name "Class_name"
```python
class Class_name(abp.Agent):
    def __init__(self, properties="default"):
        # pass the values to the parent agent
        super().__init__(properties)
        # pass the child class name to my_class 
        self.my_class = "Class_name"
```
Initialising an object of type Class_name and printing it.
```python
object = Class_name()
print(object)
```
Outcome ("Class_name <id>"):
```bash
Class_name 0
```

### Model `abp.Model()`
- ``

## Non-class methods
### `create_patches(n_x, n_y, file_directory)`
(1) Draws a grid of patches as a geopandas dataframe, (2) attaches to each an `"i"` and a `"j"` attribute consituting their location in x and y respectively and (3) saves the geopandas dataframe as a shapefile
#### Parameters
- `n_x`: int
    - number of polygons in the x-axis
- `n_y`: int
    - number of polygons in the y-axis
- `file_directory`: str
    - the full directory of the saved file (must end in .shp)
#### Returns
- geopandas object
#### Example
Create a 50 x 50 grid, save it to the python file directory and print the outcome.
```python
import abpandas as abp

grid = abp.create_patches(n_x=50, n_y=50, file_directory="50x50_grid.shp")
print(grid)
```
Outcome:
```bash
        id   i   j                                           geometry
0        1   1   1  POLYGON ((0.00000 0.00000, 1.00000 0.00000, 1....
1        2   2   1  POLYGON ((1.00000 0.00000, 2.00000 0.00000, 2....
2        3   3   1  POLYGON ((2.00000 0.00000, 3.00000 0.00000, 3....
3        4   4   1  POLYGON ((3.00000 0.00000, 4.00000 0.00000, 4....
4        5   5   1  POLYGON ((4.00000 0.00000, 5.00000 0.00000, 5....
...    ...  ..  ..                                                ...
2495  2496  46  50  POLYGON ((45.00000 49.00000, 46.00000 49.00000...
2496  2497  47  50  POLYGON ((46.00000 49.00000, 47.00000 49.00000...
2497  2498  48  50  POLYGON ((47.00000 49.00000, 48.00000 49.00000...
2498  2499  49  50  POLYGON ((48.00000 49.00000, 49.00000 49.00000...
2499  2500  50  50  POLYGON ((49.00000 49.00000, 50.00000 49.00000...

[2500 rows x 4 columns]
```




