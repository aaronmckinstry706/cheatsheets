# Python: code organization, code snippets, useful API info.

## Code Snippets

* Use `pathlib.Path` wherever possible. 

## Code Organization

* For multithreaded code, put `main` function in its own thread in main script. This makes it easier to use subcommands in the CLI. 
> ```
> import threading
> 
def main(args):
    pass

if __name__ == "__main__":
    main_thread = threading.Thread(target=main, args=(parsed_args,))
    main_thread.start()
    main_thread.join()
```

## API Info

* Use the `daemon=True` argument in `threading.Thread` constructor to make a thread quit when main thread exits. 

## Misc.

* When using a not-latest version of Python, set up an Anaconda environment with the option `python=<version>` to get a clean environment with Python version `<version>`. 

