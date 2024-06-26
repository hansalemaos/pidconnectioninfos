# Finds processes holding files/ports open, as well as all generations of children

## pip install pidconnectioninfos

### Tested against Windows 10 / Python 3.11 / Anaconda

### import

```PY 
from pidconnectioninfos import (
    get_all_children_parents_this_python_process,
    get_all_children_this_python_process,
    get_all_imagenames_and_pid,
    get_all_pids_from_image,
    get_all_pids_from_main_processes,
    get_processes_and_children,
    get_processes_and_children_and_netstat,
    is_process_alive,
    is_subprocess_alive,
    get_procs_with_open_file_pids_only,
    get_network_connections_from_process,
)
import subprocess

cmp = subprocess.Popen( # for tests below
    "ping -i 250 -n 100000 google.com",
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    shell=False,
)
import os
```
### get_all_children_parents_this_python_process()

```PY 
#     Returns all the children and parents of the current Python process.
#     :return: A dictionary containing the children and parents of the current Python process.
#              The dictionary has the following structure:
#              {
#                  "children": [list of child process IDs],
#                  "parents": [list of parent process IDs]
#              }
#     :rtype: dict

allpy = get_all_children_parents_this_python_process()
print(allpy)
(
    {
        (
            "ipython.exe",
            "ipython  -i _____t...",
            18332,
        ): {
            (
                "python.exe",
                "C:\\ProgramData\\anaco...",
                18152,
            ): {
                ("PING.EXE", "ping -i 250 -n 100000 google.com", 14984): -1,
                (
                    "WMIC.exe",
                    "wmic process where (ParentProcessId=18152) get Caption,ProcessId,CommandLine",
                    10864,
                ): -1,
            }
        }
    },
    {
        (
            "ipython.exe",
            "ipython  -i _____t...",
            18332,
        ): [
            (
                0,
                "python.exe",
                "C:\\ProgramData\\anaco...",
                18152,
            ),
            (1, "PING.EXE", "ping -i 250 -n 100000 google.com", 14984),
            (
                0,
                "WMIC.exe",
                "wmic process where (ParentProcessId=18152) get Caption,ProcessId,CommandLine",
                10864,
            ),
        ]
    },
)
```

### get_all_children_this_python_process()

```PY 
#     Returns all the (nested) children processes of the current Python process.

#     :return: A list of Process objects representing the children processes.
#     :rtype: List[Process]
```


### get_all_children_this_python_process()
```PY 
(
    {
        (
            "python.exe",
            "C:\\ProgramData\\anaconda3\\envs\\a0\\...",
            18152,
        ): {
            ("PING.EXE", "ping -i 250 -n 100000 google.com", 14984): -1,
            (
                "WMIC.exe",
                "wmic process where (ParentProcessId=18152) get Caption,ProcessId,CommandLine",
                25496,
            ): -1,
        }
    },
    {
        (
            "python.exe",
            "C:\\ProgramData\\anaconda3\\envs\\a0\\python.exe C:\\ProgramDa...",
            18152,
        ): [
            (0, "PING.EXE", "ping -i 250 -n 100000 google.com", 14984),
            (
                0,
                "WMIC.exe",
                "wmic process where (ParentProcessId=18152) get Caption,ProcessId,CommandLine",
                25496,
            ),
        ]
    },
)
```

### get_all_imagenames_and_pid()
```PY 

#     Get all image names and their corresponding process IDs.
get_all_imagenames_and_pid()
# ...
# ['chrome.exe', 16964],
# ['cmd.exe', 4176],
# ['conhost.exe', 24096],
# ['OpenConsole.exe', 544],
# ['cmd.exe', 25056],
# ['vtm.exe', 18304],
# ['cmd.exe', 25576],
# ['ipython.exe', 18332],
# ['python.exe', 18152],
#  ...
```


### get_all_pids_from_image(imagefile: str = 'chrome.exe')

```PY 
#     Get all process IDs associated with a specific image file.
#     :param imagefile: The name of the image file to search for process IDs (default is "chrome.exe")
#     :type imagefile: str
#     :return: A list of process IDs associated with the specified image file
#     :rtype: list
allchromepids = get_all_pids_from_image(imagefile="chrome.exe")

[
    18608,
    4796,
    19256,
    19004,
    11532,
    19208,
    2944,
    1160,
    19032,
    13208,
    16084,
    11688,
    10140,
    17048,
    11316,
    940,
    5600,
    6660,
    19524,
    18620,
    10060,
    10328,
    13744,
    22452,
    20752,
    22248,
    592,
    21136,
    18108,
    18492,
    18676,
    7788,
    16964,
    22932,
    19788,
]
```
### get_all_pids_from_main_processes()
```PY 
#     Gets all process IDs from the main processes and returns a list of integers.

allpids = get_all_pids_from_main_processes()
[
    0,
    4,
    64,
    132,
    468,
    688,
    776,
    784,
    888,
    312,
    488,
    496,
    952,
    8,
    504,
    1068,
    1148,
    1200,
    1272,
    1400,
    1416,
    1528,
    1536,
    1544,
    1572,
    1660,
    1712,
    1748,
    1820,
    1916,
    2036,
    1260,
    2080,
    2216,
    2224,
    2232,
    2252,
    2316,
    2376,
    2428,
    2436,
    2484,
    2492,
    2668,
    2852,
    2920,
    3008,
    1164,
    3256,
    3440,
    3448,
    3576,
    3636,
    3712,
    4220,
    4256,
    4296,
    4372,
    4460,
    4468,
    4588,
    4700,
    4724,
    4732,
    4740,
    4756,
    4768,
    4784,
    4820,
    4828,
    4876,
    4924,
    4932,
    4940,
    4948,
    4964,
    5024,
    4244,
    5124,
    5152,
    5,
    ...,
]
```

### get_processes_and_children(pids_to_search: list | tuple | int | None = None, always_ignore: tuple = (0, 4))
```PY 
#     Get the processes and their children based on the provided process IDs to search and the processes to always ignore.
#     If no IDs are provided, it will default to searching through a range of 65536 IDs.
#     The function returns two dictionaries: `resultchildren` containing the children of each process as a "family tree", and `resultchildren_flat` containing a flattened version of the children relationship.

procs_and_kids = get_processes_and_children(allchromepids, always_ignore=(0, 4))
{
    (
        "chrome.exe",
        '"C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe"',
        1968,
    ): {
        (
            "chrome.exe",
            '"C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe" --type=crashpad-handler ...")',
            1968,
        ): -1
    }
}
```
### get_processes_and_children_and_netstat(pids_to_search: list | tuple | int | None = None, always_ignore: tuple = (0, 4))
```PY 
#     Retrieves a tree of processes, their children, and netstat connection information.
#     Args:
#         pids_to_search (Optional[List[int]]): A list of process IDs to search for. Defaults to None.
#         always_ignore (Tuple[int]): A tuple of process IDs to always ignore. Defaults to (0, 4).
#     Returns:
#         Tuple[Dict[int, Dict[str, Any]], List[Dict[str, Any]], Dict[int, List[List[str]]], Dict[int, List[List[str]]]]:
#             - A tree of processes and their children, represented as a dictionary.
#             - A flat list of processes and their children, represented as a list of dictionaries.
#             - A dictionary of established connections, with the process ID as the key and a list of connection information as the value.
#             - A dictionary of all connection data, with the process ID as the key and a list of connection information as the value.


procs_and_kids_netstat = get_processes_and_children_and_netstat(
    allchromepids, always_ignore=(0, 4)
)
{
    688: [
        ("TCP", "127.0.0.1:5835", "0.0.0.0:0", "LISTENING", 688),
        ("TCP", "127.0.0.1:5835", "127.0.0.1:53324", "ESTABLISHED", 688),
        ...,
        ("UDP", "0.0.0.0:61382", "*:*", 688),
        ("UDP", "[::]:61382", "*:*", 688),
    ],
    13380: [
        ("TCP", "127.0.0.1:5845", "0.0.0.0:0", "LISTENING", 13380),
        ("TCP", "127.0.0.1:5845", "127.0.0.1:53334", "ESTABLISHED", 13380),
        ...,
        ("UDP", "[::]:58688", "*:*", 13380),
    ],
    5140: [
        ("TCP", "127.0.0.1:31100", "0.0.0.0:0", "LISTENING", 5140),
        ("TCP", "127.0.0.1:49668", "127.0.0.1:31104", "ESTABLISHED", 5140),
        ("TCP", "127.0.0.1:49680", "127.0.0.1:31104", "ESTABLISHED", 5140),
    ],
    5208: [
        ("TCP", "127.0.0.1:49726", "127.0.0.1:65001", "ESTABLISHED", 5208),
        ("TCP", "127.0.0.1:65001", "0.0.0.0:0", "LISTENING", 5208),
        ("TCP", "127.0.0.1:65001", "127.0.0.1:49726", "ESTABLISHED", 5208),
        ("UDP", "0.0.0.0:59435", "*:*", 5208),
        ...,
        ("UDP", "[::]:59436", "*:*", 5208),
        ("UDP", "[::1]:5353", "*:*", 5208),
    ],
    9844: [
        ("TCP", "127.0.0.1:49728", "0.0.0.0:0", "LISTENING", 9844),
        ("TCP", "127.0.0.1:49728", "127.0.0.1:49737", "ESTABLISHED", 9844),
        ("UDP", "127.0.0.1:10010", "*:*", 9844),
    ],
    13932: [("TCP", "127.0.0.1:49737", "127.0.0.1:49728", "ESTABLISHED", 13932)],
}
```

### is_process_alive(pid: int)

```PY 
#     A function that checks if a process with a given Process ID is alive.
#     Parameters:
#         - pid: int, the Process ID of the process to check.
#     Returns:
#         - tuple: (bool, tuple) where the boolean indicates if the process is alive, and the tuple contains information about the process.
```

### is_process_alive(pid=os.getpid())
```PY 
(True, ("python.exe", "C:\\ProgramData\\anaconda3\\envs\\a0\\python.exe ", ...))
```


### is_subprocess_alive(cmp: subprocess.Popen)
```PY 
#     Check if a subprocess is still alive.

#     Parameters:
#         cmp (subprocess.Popen): The subprocess to check.

#     Returns:
#         bool: True if the subprocess is still alive, False otherwise.

is_subprocess_alive(cmp=cmp)
True
```

### get_procs_with_open_file_pids_only(path):
```PY 
#     """
#     Get the list of process IDs that have an open file at the specified path.

#     Args:
#         path (str): The path of the file.

#     Returns:
#         list: A list of process IDs that have the file open.
a = get_procs_with_open_file_pids_only(path=r"C:\uber.xlsx")
[18280]

b = get_procs_with_open_file_pids_only(path=r"C:\planotreinar.mp4")
[17120]
(
    {
        (
            "vlc.exe",
            '"C:\\Program Files\\VideoLAN\\VLC\\vlc.exe" --started-from-file "C:\\planotreinar.mp4"',
            25220,
        ): -1
    },
    {
        (
            "vlc.exe",
            '"C:\\Program Files\\VideoLAN\\VLC\\vlc.exe" --started-from-file "C:\\planotreinar.mp4"',
            25220,
        ): [(0,)]
    },
)


(
    {
        (
            "EXCEL.EXE",
            '"C:\\Program Files\\Microsoft Office\\Root\\Office16\\EXCEL.EXE" "C:\\tempTest2.csv"',
            2928,
        ): {("splwow64.exe", "C:\\WINDOWS\\splwow64.exe 12288", 10112): -1}
    },
    ...,
)
```

### get_processes_and_children_and_netstat / get_processes_and_children
```PY 
procs_and_kids_netstat = get_processes_and_children_and_netstat(a, always_ignore=(0, 4))
procs_and_kids = get_processes_and_children(b, always_ignore=(0, 4))
"""
Retrieves a tree of processes, their children, and netstat connection information.

Args:
    pids_to_search (Optional[List[int]]): A list of process IDs to search for. Defaults to None.
    always_ignore (Tuple[int]): A tuple of process IDs to always ignore. Defaults to (0, 4).

Returns:
    Tuple[Dict[int, Dict[str, Any]], List[Dict[str, Any]], Dict[int, List[List[str]]], Dict[int, List[List[str]]]]:
        - A tree of processes and their children, represented as a dictionary.
        - A flat list of processes and their children, represented as a list of dictionaries.
        - A dictionary of established connections, with the process ID as the key and a list of connection information as the value.
        - A dictionary of all connection data, with the process ID as the key and a list of connection information as the value."""

ports = [5555, 5037, 17476, 5835, 5845, 443, 80]
fatr = get_network_connections_from_process(ports=ports, always_ignore=(0, 4))
print(fatr)
{
    10348: {
        "family_tree": {
            ("adb.exe", "adb -L tcp:5037 fork-server server --reply-fd 616", 10348): -1
        },
        "flat_tree": {
            ("adb.exe", "adb -L tcp:5037 fork-server server --reply-fd 616", 10348): [
                (0,)
            ]
        },
        "found_ports": {5845, 5835, 5037},
    },
    688: {
        "family_tree": {
            (
                "HD-Player.exe",
                '"C:\\\\Program Files\\\\BlueStacks_nxt\\\\HD-Player.exe" "--instance" "Rvc64_28"',
                688,
            ): -1
        },
        "flat_tree": {
            (
                "HD-Player.exe",
                '"C:\\\\Program Files\\\\BlueStacks_nxt\\\\HD-Player.exe" "--instance" "Rvc64_28"',
                688,
            ): [(0,)]
        },
        "found_ports": {5835, 443},
    },
    13380: {
        "family_tree": {
            (
                "HD-Player.exe",
                '"C:\\\\Program Files\\\\BlueStacks_nxt\\\\HD-Player.exe" "--instance" "Rvc64_29"',
                13380,
            ): -1
        },
        "flat_tree": {
            (
                "HD-Player.exe",
                '"C:\\\\Program Files\\\\BlueStacks_nxt\\\\HD-Player.exe" "--instance" "Rvc64_29"',
                13380,
            ): [(0,)]
        },
        "found_ports": {443, 5845},
    },
    5440: {
        "family_tree": {
            (
                "svchost.exe",
                "C:\\WINDOWS\\system32\\svchost.exe -k netsvcs -p -s WpnService",
                5440,
            ): -1
        },
        "flat_tree": {
            (
                "svchost.exe",
                "C:\\WINDOWS\\system32\\svchost.exe -k netsvcs -p -s WpnService",
                5440,
            ): [(0,)]
        },
        "found_ports": {443},
    },
    2304: (...,),
}
```
