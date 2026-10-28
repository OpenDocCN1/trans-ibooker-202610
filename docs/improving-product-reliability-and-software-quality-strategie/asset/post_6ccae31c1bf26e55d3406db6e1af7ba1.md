
- Defects should be assignable to engineers. Any given defect might be assigned to a different engineer depending on its state.
- Defects should be associated with specific products and components of those products.
- The DTS should support a workflow such that the state of the defect and the assignment of the defect should automatically transition when action is taken on the defect. For example, when a defect is opened, it might be automatically assigned to a specific development engineer; when it is fixed, it might automatically be assigned to the appropriate SQA engineer.
- The DTS should provide notifications to the assigned engineers when a defect is opened or transitions state. Additionally, the DTS should allow notifications to additional interested parties, such as relevant managers.
- The DTS should allow a user to add comments and attachments (such as logs and screenshots) to the bug report.
- The DTS should provide a rich set of reports to be used for determining defect counts, defect open and closure rates, and root cause analysis.

Deploying and using a DTS will improve productivity by making sure each engineer knows there are defects to fix in a timely fashion. Additionally, information can be added to the bug report to facilitate the repair of the defect. For example, when a defect is opened, the report could specify the steps required to duplicate the defect. This will make it easier to reproduce and thus fix and test it.

A good DTS is required to track the quality of a product. Once a DTS is put in place, it must be required that engineers enter bug reports in the DTS and use the DTS to track the resolution of each defect.

## 12.8 Software and Hardware Integration

One of the best ways to improve productivity is to create reusable software. Common functionality can be coded in such a way that it can be used in project after project without making changes. Not only does this result in effort savings for subsequent projects, but it also improves product quality by relying on tested proven code. New code will have new bugs and must be thoroughly tested before release. Reused code has already been tested and shown to work in production. Additional SQA can be limited to merely regression testing.

Code reusability can be applied to hardware and software integration. Typically, hardware exposes a register map for software to use to interact with the hardware. However, registers often change from one hardware implementation to the next. This poses a challenge for reuse; either the code has to change with each register map change or the hardware designers are constrained by leaving registers unchanged. The solution to this problem is to create a thin layer of software called a hardware abstraction layer (HAL). HALs expose a functional interface for the hardware at the function or method level which can be called by higher level software. Typically, the software and the hardware teams jointly design that functional interface. HALs hide the hardware implementation details from the upper level code, so that the higher-level application code can be reused from project to project without worrying about the register assignments of the actual hardware. The only code that has to change when the hardware implementation changes is the HAL code itself and generally the changes to this code are easy to test. In fact, creating and maintaining HALs also makes it more efficient to perform design verification (DV) on the hardware itself by allowing the DV code to be reused as well as the application code. Thus, the hardware team will also be able to more efficiently provide more stable hardware to the software team when the hardware and software integration tasks begin.

Generally, the hardware teams create and maintain the HALs. Since the hardware developers may be changing the register maps from project to project, or even during the course of developing a particular component, it is most efficient for them to also change the implementation of the HALs in conjunction with the hardware changes.

For maximum payback, HALs should be created for functions that are common to many hardware implementations. This would include generic common functionality such as buses and communications interfaces like USB, JTAG, UART, and SPI. For functionality common to your hardware types, HALs would also be highly beneficial as well. For example, developing HALs to control motors, perform pulse width modulation (PWM), control power supplies, and so on would probably all deliver good ROIs.

A sample HAL interface to read from or write to a UART is shown in Figure 12.4.

```
// bool UartRead() reads data from the UART.
// returns TRUE if successful, FALSE if read fails.
// int dataLength: Size of read buffer.
// out String receiveBuffer: The data read from the UART.
// out int returnLength: The length of the returned data.
// out String errorMessage: Error message if call fails. Empty if successful.
//
bool UartRead(int dataLength, out String receiveBuffer, out returnLength, out String errorMessage);

// bool UartWrite() writes data to the UART.
// returns TRUE if successful, FALSE if write fails.
// int dataLength: Size of data to write.
// out String writeBuffer: The data to write to the UART.
// out String errorMessage: Error message if call fails. Empty if successful.
//
bool UartWrite(int dataLength, out String writeBuffer, out String errorMessage);
```

Figure 12.4 Sample UART HAL code.

Here are three guidelines to follow to create good HALs:

- 1. HAL interfaces should be named clearly with explicit names so that they are easy to understand and for the higher-level software to consume. If HALs’ function names are cryptic, they can confuse the consumer of the HALs.
- 2. Most HALs should support parameters for initialization and or to pass values between the software and the hardware.
- 3. HALs should be kept at the highest level of abstraction possible without incorporating any application code level logic.

Sometimes specific registers need to be read or written by higher-level code. Often, this is the case with DV code where a test is performed and then registers are read to confirm they are correct. A HAL can be provided to generically read or write a register. Even though code to read or write a register is easy to write, repetitively implementing such code is an opportunity to introduce defects.

Reusing code is one of the best ways to improve software development efficiency and software quality. One of the most effective techniques for providing reuse of software that interface with hardware is to create HALs to isolate the software from the hardware. The benefits of using HALs include less effort to port software between similar hardware, less effort to test implementations of similar hardware, and better-quality software.

## References

- 1. Carroll, L. (1865). Alice's Adventures in Wonderland, first printed. New York Millennium 2014.
- 2. ISO/IEC/IEEE 29148 Systems and Software Engineering – Life Cycle Processes – Requirements Engineering, 2011.
- 3. Hunt, A. and Thomas, D. (2000). The Pragmatic Programmer: From Journeyman to Master. Addison Wesley Longman, Inc.
- 4. Stockton, N. (2014). What's Up with That?: Why It's so Hard to Catch Your Own Typos. Wired.

## Further Reading

Cockburn, A. (2001). Writing Effective Use Cases. Addison-Wesley.
Doxygen home page, http://www.stack.nl/~dimitri/doxygen
Freedman, D.P. and Weinberg, G.M. (1990). Handbook of Walkthroughs, Inspections, and Technical Reviews: Evaluating Programs, Projects, and Products. Dorset House.
Microsoft Inc., How to: Insert XML comments for documentation generation, https://docs.microsoft.com/en-us/visualstudio/ide/reference/generate-xml-documentation-comments
Microsoft, Inc., Recommended Tags for Documentation Comments (C# Programming Guide), https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/xml-doc/recommended-tags-for-documentation-comments
Wiegers, K. and Beatty, J. (2013). Software Requirements, 3e. Microsoft Press.
