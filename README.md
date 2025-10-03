# Seiya OS

A minimalist operating system kernel implementation for RISC-V 32-bit architecture.

## Features

- **Process Management**: Multi-process support with context switching and scheduling
- **Memory Management**: Page table implementation with virtual memory support
- **System Calls**: Complete syscall interface (getchar, putchar, readfile, writefile, exit)
- **File System**: Tar-based filesystem with read/write capabilities
- **Disk I/O**: VirtIO block device driver for disk operations
- **User Mode**: Separate kernel and user space with proper privilege separation
- **Interactive Shell**: Simple command-line interface for file operations

## Architecture

- **Target**: RISC-V 32-bit (RV32IMAFDC)
- **Paging**: SV32 virtual memory scheme
- **Boot**: OpenSBI firmware
- **Emulation**: QEMU virt machine

## Project Structure

```
.
├── kernel.c          # Kernel implementation
├── kernel.h          # Kernel headers and definitions
├── kernel.ld         # Kernel linker script
├── shell.c           # User-space shell application
├── user.c            # User-space system call wrappers
├── user.h            # User-space headers
├── user.ld           # User-space linker script
├── common.c          # Shared utility functions (memset, memcpy, etc.)
├── common.h          # Shared headers
├── run.sh            # Build and run script
└── disk/             # Filesystem contents (tar archive)
```

## Building and Running

### Prerequisites

- **QEMU**: `qemu-system-riscv32`
- **Compiler**: Clang with RISC-V target support
- **Tools**: llvm-objcopy

### Build and Run

```bash
./run.sh
```

The script will:
1. Compile the user-space shell application
2. Convert it to a binary blob
3. Compile the kernel with embedded shell
4. Create a tar archive from the disk directory
5. Launch QEMU with the kernel and disk image

## Shell Commands

- `hello` - Print a greeting message
- `readfile` - Read contents of hello.txt
- `writefile` - Write a test message to hello.txt
- `exit` - Exit the shell

## Technical Details

### Memory Layout

- **Kernel Space**: 0x80200000+
- **User Space**: 0x01000000 (USER_BASE)
- **Page Size**: 4KB (0x1000 bytes)

### System Calls

| System Call | ID | Description |
|-------------|-----|-------------|
| SYS_PUTCHAR | 1 | Write a character to console |
| SYS_GETCHAR | 2 | Read a character from console |
| SYS_EXIT | 3 | Terminate the current process |
| SYS_READFILE | 4 | Read file from filesystem |
| SYS_WRITEFILE | 5 | Write file to filesystem |

### File System

Uses the USTAR tar format as a simple filesystem. Files are loaded into memory at boot and changes are written back to the disk image on write operations.

## Implementation Highlights

- **Context Switching**: Assembly-based context switching with callee-saved register preservation
- **Page Tables**: Two-level page table implementation for SV32
- **VirtIO**: Block device driver following VirtIO specification
- **Exception Handling**: Trap handling for syscalls and exceptions
- **User Pointer Access**: Proper handling of user-space pointers with SUM bit

## License

Educational project - free to use and modify.

