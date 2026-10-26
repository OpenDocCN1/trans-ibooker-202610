```
std::cout << "Successfully opened file for reading" << std::endl;

CloseHandle(hFile);

return 0;
}
```

We use the same operations to perform reading or writing to files, that is, **ReadFile** and **WriteFile**, with the only difference that the number of reads or wrote bytes are set to NULL and we must pass a valid and unique **OVERLAPPED** object. Let's consider what the structure of the OVERLAPPED object is:

```
typedef struct _OVERLAPPED {
DWORD Internal; // for error code
DWORD InternalHigh; // for number of read bytes
DWORD Offset;
DWORD OffsetHigh;
HANDLE hEvent; // handle to an event
} OVERLAPPED, *LPOVERLAPPED;
```

The Internal member is set to **STATUS_PENDING**, which means that the operation hasn't started yet. The number of read or wrote bytes will be written into the **InternalHigh** member. **Offset** and **OffsetHigh** are ignored in asynchronous operations. The **hEvent** member is used for receiving events about the completion of the asynchronous operation.

> **Note**
> 
> The order of the I/O operations is not guaranteed, so you cannot rely on this. If you plan to write to a file at one place, and read from a file at another place, you cannot rely on the order.