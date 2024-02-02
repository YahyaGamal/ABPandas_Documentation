# ABPandas Documentation
AB
## Installation
Install using pip as follows
```bash
pip install abpandas
```
You can also download a wheel file from [PYPI](https://pypi.org/project/abpandas/#files). To install the wheel file first install $wheel$ as follows
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

### abp.Agent
properties: dictionary, default= {}
    the properties of the agent object
__id: int
    a unique id given to each agent (accessed by the Model class when creating the Agent)
location_index: int, default= None
    the index to which the agent is assigned (accessed by the Model class when creating or moving the Agent)
