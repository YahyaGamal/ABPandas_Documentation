# ABPandas Documentation
1. [Installation](#installation)  
2. [Classes](#classes)  
    2.1. [Agent](#agent-abpagentproperties)  
    2.2. [Model](#model-abpmodel) \
    [create_agent](#create_agentproperties)  [create_agents](#create_agentproperties) [add_agent](#add_agentagent-loc_index)  [add_agents](#add_agentagent-loc_index)  [agents_with_id](#agents_with_idid)  [agents_with_props](#agents_with_propscondition)  [agents_at](#agents_atloc_index)  [move_agent](#move_agentagent-new_loc_index)  [move_agents](#move_agentsagents-new_loc_index)  [remove_agent](#remove_agentagent)  [remove_agents](#remove_agentsagents)  [save_space](#save_spacefile_directory)  [index_at_ij](#index_at_iji-j)  [indices_in_radius](#indices_in_radiuscentre-radius-outline_only-return_patches)  [indexes_in_radius](#indices_in_radiuscentre-radius-outline_only-return_patches)  [distance](#distancea-b)
3. [Non-class methods](#non-class-methods)

# Installation
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

# Classes

## Agent `abp.Agent(properties)`
#### Inputs
- `properties`: dictionary, default= {}  
    - the properties of the agent object (must be a dictionary input for the abp.Model methods to function properly)
#### Attributes
- `Agent.props`: dict, default= {}
    - the properties of the agent (assigned as the `properties` input attribute at initialising the agent)
- `Agent.location_index`: int, default= None  
    - the index to which the agent is assigned (accessed by the Model class when creating or moving the Agent)
- `Agent.my_class`: str
    - the name of the agent class in the model (must be defined as a string in the `__init__` function of the child class definition)
#### Example 1
Create an agent object with a wealth property, print the agent and print its properties.
```python
my_agent = abp.Agent(properties={"Wealth": 100})
print(my_agent)
print(my_agent.props)
print(my_agent.props["Wealth"])
```
Outcome:
```bash
Agent 0
{"Wealth": 100}
100
```

#### Example 2
Child class definition of name "Investor"
```python
class Investor(abp.Agent):
    def __init__(self, properties="default"):
        # pass the values to the parent agent
        super().__init__(properties)
        # pass the child class name to my_class 
        self.my_class = "Investor"
```
Initialising an object of type Class_name and printing it.
```python
object = Investor()
print(object)
```
Outcome:
```bash
Investor 0
```

---

## Model `abp.Model(space, agents)`
#### Inputs
- `space`: geopandas object
    - a spatial map with polygons read as a geopandas object
- `agents`: list of Agent objects, default= []
    - a list of agents currently in the model
#### Attributes
- `Model.space`: geopandas object
    - assigned as the input `space`
- `Model.agents`: list of Agent objects, default= []
    - assigned as the input `agents`
- `<Agent.my_class>s`: list of agents of a specific class
    - initialised on introducing an new agent sub-class
    - e.g., adding an house `Agent` with `Agent.my_class="house"` creates a model attribute `Model.houses`
    - note that the model adds an `"s"` to the str in `Agent.my_class` regardless of gramatical considerations
#### Example
Create a model with an investor and a generic agent, then print Model.agents and Model.invetors.
```python
# create a model with a 50 x 50 grid and an investor and a generic agent
grid= abp.create_patches(50, 50, "50x50_grid.shp")
model= abp.Model(space=grid, agents=[Investor(), abp.Agent()])
# print agents
print(model.agents)
# print investors
print(model.investors)
```
Outcome:
```bash
[Investor 0, Agent 1]
[Investor 0]
```
Check defining a child class from abp.Agent class [here](#example-2).

---

### `create_agent(properties)`
### `create_agents(N, properties`
Create an agent or an `N` number of agents with an input dictionary of properties.
#### Parameters
- `N`: int
    - the number of agents
- `properties`: dictionary, default= {}
    - a dictionary of the properties of agents
#### Returns
- list of Agent objects
#### Example
Create agents in a model with a Wealth property.
```python
# create a model with a 50
grid= abp.create_patches(50, 50, "50x50_grid.shp")
model= abp.Model(space=grid)
# add one agent to the model, print the agents in the model and their wealth
model.create_agent(properties={"Wealth": 100})
print(model.agents)
print([agent.props["Wealth"] for agent in model.agents])
# add two agents to the model, print the agents in the model and their wealth
model.create_agents(N=2, properties={"Wealth": 200})
print(model.agents)
print([agent.props["Wealth"] for agent in model.agents])
```
Outcomes:
```bash
[Agent 0]
[100]
[Agent 0, Agent 1, Agent 2]
[100, 200, 200]
```

---

### `add_agent(agent, loc_index)`
### `add_agents(agents, loc_index)`
Add agent(s) to the model and attach a unique ID to each in the model.
#### Parameters
- `agent`: Agent
- `agents`: list of Agent objects
    - previously created agent(s)
- `loc_index`: int, default= None
    - the index of the patch at which the agent is places
#### Example
Add agents to a model and print the agents in the model
```python
# create a model with a 50
grid= abp.create_patches(50, 50, "50x50_grid.shp")
model= abp.Model(space=grid)
# add one agent to the model
model.add_agent(agent=abp.Agent())
print(model.agents)
# add two agents to the model
model.add_agents(agents=[abp.Agent(), abp.Agent()])
print(model.agents)
```
Outcome:
```bash
[Agent 0]
[Agent 0, Agent 1, Agent 2]
```

---

### `agents_with_id(id)`
Search for agents by id
#### Parameters
- `id`: int or list
    - the id(s) of the agent to search for
#### Returns
an Agent object (if id is int) or a list of Agent objects (if id is list)

---

### `agents_with_props(condition)`
Search for agents based on their properties using a conditional function
#### Parameters
- `condition`: function
    - a function taking in an abp.Agent and returning true if the condition is satisfied
#### Returns
list of Agent objects fulfilling the condition
#### Example
Find bankrupt investors with `props["wealth"] == 0`
```python
# create a model with a 50 x 50 grid and an investor and a generic agent
grid= abp.create_patches(50, 50, "50x50_grid.shp")
model= abp.Model(space=grid, agents=[Investor({"wealth": 0}), abp.Agent({"wealth": 0}), Investor({"wealth": 100})])
# function checking if agent is bankrupt (has 0 wealth)
def bankrupt(agent):
    if agent.props["wealth"] <= 0: return True
    else: return False
# find the agents that are bankrupt in the model
bankrupt_agents = model.agents_with_props(bankrupt)
print(bankrupt_agents)
```
Outcome:
```bash
[Investor 0, Agent 1]
```

---

### `agents_at(loc_index)`
Find agents based on their location index
#### Parameters
- `loc_index`: int
    - the index of the polygon in the space
#### Returns
- list of Agent objects located in input index
#### Example
Add agents at indices 100 and 200, and find agents at index 100.
```python
# create a model with a 50 x 50 grid and an investor and a generic agent
grid = abp.create_patches(50, 50, "50x50_grid.shp")
model = abp.Model(space=grid)
# add two agents at two different location indices
model.add_agents(agents=[abp.Agent(), abp.Agent()], loc_index=100)
model.add_agent(agent=abp.Agent(), loc_index=200)
# find the agents at location index 100
agents_at_loc = model.agents_at(loc_index=100)
print(model.agents)
print(agents)
```
Outcome:
```bash
[Agent 0, Agent 1, Agent 2]
[Agent 0, Agent 1]
```

---

### `move_agent(agent, new_loc_index)`
### `move_agents(agents, new_loc_index)`
Move an agent to a new location
#### Parameters
- `agent(s)`: an Agent object or a list of Agent objects 
    - the agent(s) to move to a new location
- `new_loc_index`: int
    - the index of the polygon in the sapce to which agents will move

---

### `remove_agent(agent)`
### `remove_agents(agents)`
Removes agents from the model
#### Parameters
- `agent(s)`: an Agent object or a list of Agent objects 
    - the agents to remove from the model

---

### `save_space(file_directory)`
Saves the space to a shape file or a pkl file
#### Parameters
- `file_directory`: str
    - the full directory of the file to be saved (must end in .shp or .pkl)

---

### `index_at_ij(i, j)`
Finds the patches based on their i and j location (i and j are the indices of a patch in x-direction and y-direction respectively).  
i and j are created during the create_patches() function.
#### Parameters
- `i`: int
    - the index of the patch in x-direction (starts from 1)
- `j`: int
    - the index of the patch in y-direction (starts from 1)
#### Returns
- int index of the polygon in the abpandas.space geodataframe

---

### `indices_in_radius(centre, radius, outline_only, return_patches)`
### `indexes_in_radius(centre, radius, outline_only, return_patches)`
finds the patches (polygons) within a given radius
#### Parameters
- `centre`: Agent or int or a shapely polygon
    - central agent, patch index or shapely polygon
- `radius`: int or float
    - the radius from the centre
- `outline_only`: bool, default=False
    - limit the outcome to the outline of the create_space generated polygons
- `return_patches`: bool, default=False
    - return a list of shapely geometries instead of indices
#### Returns
- list of indeces (if return_patches=False)
    - index of the polygons within the radius in Model.space
- list of shapely polygons (if return_patches=True)
    - the polygons within the radius in Model.space

---

### `distance(a, b)`
finds the distance between patches and agents
#### Parameters
- `a`: Agent, index or shapely polygon
    - the first object
- `b`: Agent, index or shapely polygon
    - the second object
#### Returns
- float distance (units depends on shapefile)

---

# Non-class methods
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

---




