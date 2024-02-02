# ABPandas Documentation
AB
## Installation
Install using pip as follows
```bash
pip install abpandas
```
You can also download a wheel file from [PYPI](https://pypi.org/project/abpandas/#files). To install the wheel file first install `wheel` as follows
```bash
pip install wheel
```
Then install the wheel file as follows
```bash
pip install abpandas-VERSION-py3-none-any.whl
```
Once installed, you can no wimply import `ABPandas` into your python script as `abp`
```python
import ABPandas as abp
```

## Classes

### `abp.Agent(properties)`
- properties: dictionary, default= {}  
    - the properties of the agent object 
- location_index: int, default= None  
    - the index to which the agent is assigned (accessed by the Model class when creating or moving the Agent)
- my_class: str
    - the name of the agent class in the model (must be defined as a string in the `__init__` function of the child class definition)

An eample of a child class initialisation
```python
class Child_class_name(abp.Agent):
    def __init__(self, properties="default"):
        # pass the values to the parent agent
        super().__init__(properties)
        # pass the child class name to my_class 
        self.my_class = "Child_class_name"
```

### `abp.Space()`
