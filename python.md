# Python: code organization, code snippets, useful API info.

## Resources

* [log format string](https://docs.python.org/3/library/logging.html#logrecord-attributes)

## Code, etc.

* Use `pathlib.Path` wherever possible. 

* For multithreaded code, consider putting primary work function in its own thread in main script. This can make it easier to coordinate with other threads as necessary--for example, if one thread does heavy processing while the other listens for input commands, as is the case in neural net training code. 
```python
import threading

def work(args):
    print(args)

# ...

def main():
    parsed_args = get_args()
    main_thread = threading.Thread(target=main, args=(parsed_args,))
    main_thread.start()
    main_thread.join()

if __name__ == "__main__":
    main()
```

* Always put the main code in a function. 

* Always define a `get_args()` function to get the script arguments. 

* For subcommands, where you need to know what subcommand was called:

```python
def cmd1(args):
    print(args)

def cmd2(args):
    print(args)

def get_args():
    arg_parser = argparse.ArgumentParser(dest='subparser_name')
    subparsers = arg_parser.add_subparsers()
    
    cmd1_parser = subparsers.add_subparser(help='cmd1 help')
    cmd1_parser.add_argument('--bla')
    
    cmd2_parser = subparsers.add_subparser(help='cmd2 help')
    cmd2_parser.add_argument('--bla-bla')
    
    parsed_args = arg_parser.parse_args()
    
    return parsed_args

if __name__ == "__main__":
    parsed_args = get_args()
    
    # Do stuff based on the selected subcommand.
    # ...
    
    if parsed_args.subparser_name == 'cmd1':
        cmd1(parsed_args)
    elif parsed_args.subparse_name == 'cmd2':
        cmd2(parsed_args)
```

* For subcommands, where the logic of each command is entirely separate:

```python
def cmd1(args):
    print(args)

def cmd2(args):
    print(args)

def get_args():
    arg_parser = argparse.ArgumentParser()
    subparsers = arg_parser.add_subparsers()
    
    cmd1_parser = subparsers.add_subparser(help='cmd1 help')
    cmd1_parser.add_argument('--bla')
    cmd1_parser.set_defaults(func=cmd1)
    
    cmd2_parser = subparsers.add_subparser(help='cmd2 help')
    cmd2_parser.add_argument('--bla-bla')
    cmd2_parser.set_defaults(func=cmd2)
    
    parsed_args = arg_parser.parse_args()
    
    return parsed_args

if __name__ == "__main__":
    parsed_args = get_args()
    parsed_args.func(parsed_args)
```

* Logging in main module:

```python
import logging
import sys

# ...other imports...

LOGGER = logging.getLogger(__name__)

# ...

if __name__ == '__main__':
    stream_handler = logging.StreamHandler(sys.stdout)
    stream_handler.setLevel(logging.DEBUG)
    LOGGER.addHandler(stream_handler)
    LOGGER.setLevel(logging.INFO) # Or whatever application-wide logging level you want.
```

* Logging in non-main modules:

```python
import logging

# ...other imports...

LOGGER = logging.getLogger(__name__)
stream_handler = logging.StreamHandler(sys.stdout)
stream_handler.setLevel(logging.DEBUG)
LOGGER.addHandler(stream_handler)
LOGGER.setLevel(logging.INFO) # Or whatever application-wide logging level you want.

# ...the rest of the code...
```

* Use the `daemon=True` argument in `threading.Thread` constructor to make a thread quit when main thread exits. 

* When using a not-latest version of Python, set up an Anaconda environment with the option `python=<version>` to get a clean environment with Python version `<version>`. 

* For putting additional restructions on command line argument values, beyond their type:

```python
arg_parser = argparse.ArgumentParser()
arg_parser.add_argument('--restricted-string', '-r', default='default')

parsed_args = arg_parser.parse_args()

if arg_parser.restricted_string not in ['allowed', 'values']:
    arg_parser.error(arg_parser.restricted_string + ' is not an allowed value for --restricted-string.')

return 
```
