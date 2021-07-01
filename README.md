# mvr-snippets

Snippets for Cisco NSO MVR-v2 framework

## Features

Generates the snippets for the MVR-v2 code

### MVR imports

Imports the required parent classes for the workflow code

prefix: `mvrimport`

```python
from typing import Dict, List, Union
from mvr_v2.common.utils import (Correlator, Dict2, Dx, MvrInit, MvrWf,
                                 TemplateEngine, XQuery)
```

### MVR main workflow class

This is where the main workflow of the data extraction, correlation and template application is performed. These should be performed under the ***execute*** method

prefix: `mvrwf`

```python
from typing import Dict, List, Union
from mvr_v2.common.utils import (Correlator, Dict2, Dx, MvrInit, MvrWf,
                                 TemplateEngine, XQuery)

class Main(MvrWf):
    """
    Main Execution Flow. This is where the logic resides.
    """

    def execute(self, _input_obj):
        """
        This method call triggers the execution
        """
        _m = MvrInit()
        input_obj = _m >> _input_obj
        return _m
```

### Create a Dx Class

Data extraction class. Number of args 

prefix: `mvrdx`

```python
class Classname(Dx):
    def __init__(self, *args) -> None:
        pass

    def data_source(self, *args) -> XQuery:
        self.q.xpath = f"/devices/device[name={arg1!r}]/config/ip/vrf[name={arg2!r}]"
        self.q.params = {
            "param-name": "relative-path-to-desired-node",
        }
        return self.q

    def compute_vars(self, _p: Dict2) -> Union[Dict, Dict2]:
        return _p

    def devices_to_be_synced(self, _p: Dict2) -> List:
        return []
```

### Create a Correlator Class

MVR Correlator Class. Accesses all the Dx datasets and correlates, producing one or more payload instances.

prefix: `mvrcorrelator`

```python
class Classname(Correlator):
    def execute(self, data):
        payload = []
        for _i in data.primary_data_set:
            _instance = self.payload_instance()
            # list comprehension filtering 
            data_set_1 = []
            data_set_2 = []
            data_set_3 = []
            # the add method accepts a Dict, Dict2 or a List object
            _instance.add(_i)
            _instance.add(data_set_1)
            _instance.add(data_set_2)
            _instance.add(data_set_3)
            # or
            # _instance.add_multiple([_i,data_set_1,data_set_2,data_set_3])
            payload.append(_instance)
        return payload
```

### Create a TemplateEngine Class

MVR Correlator Class. Accesses all the Dx datasets and correlates, producing one or more payload instances.

prefix: `mvrtemplate`

```python
class Classname(TemplateEngine):
    def settings(self):
        self.templates = ["example"]

    def format_single_commit_comment(self, _input) -> str:
        # return _input.device_name
        return 'test'

    def format_splitted_commit_comment(self, _single_payload_instance) -> str:
        #return _single_payload_instance.interface_id
        return 'test'
```


## Release Notes

### 0.0.3

Initial release of mvr-v2 snippets
