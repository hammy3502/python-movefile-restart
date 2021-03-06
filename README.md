# python-movefile-restart

A small library to move, delete, and rename files at Windows restart time.

## Installation

`pip install movefile-restart` or `pip3 install movefile-restart`, depending on your configuration of Python and Pip.

## Usage

To import, use `import movefile_restart`

From there, you have a couple functions at your disposal:

`movefile_restart.DeleteFile(file_path, check_conflicts=True)`: Queues `file_path` for deletion.*

`movefile_restart.MoveFile(from_path, to_path, check_conflicts=True)` or `movefile_restart.RenameFile(from_path, to_path)`: Moves the file from `from_path` to `to_path`.*

`movefile_restart.GetFileOperations()`: Get a list of tuples containing the source and destination of all file movings queued.

`movefile_restart.PrintFileOperations()`: Print a list of file operations that are scheduled to occur during reboot.

`movefile_restart.RemoveFileOperation(file_op_index)`: Remove a file operation based on its index from `movefile_restart.GetFileOperations()`.

`movefile_restart.CheckPermissions()`: Check for read/write permissions to the registry keys needed for this library.

*: For both of these functions, the `check_conflicts` parameter determines whether or not to perform checks when moving/deleting to make sure if it can be performed successfully in the case of a conflict. It only checks if there is initially a problem (the file being deleted doesn't exist, the source of the file being moved doesn't exist, or the destination file of a move already exists). Setting `check_conflicts` to False when calling these functions will skip all of these checks.

## Current Limitations

* Due to using the Windows Registry for handling these kinds of operations, no other operating system is supported, nor is there planned support.
* Some cases can occur where a move/delete can fail. For example, queueing a deletion of file A then queueing a move from file A to file B isn't currently checked for, and will result in the move not occuring (as file A will be deleted before it can be moved).