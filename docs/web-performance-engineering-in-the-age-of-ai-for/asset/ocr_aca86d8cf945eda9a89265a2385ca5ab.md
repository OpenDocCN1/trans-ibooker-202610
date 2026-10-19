# Chrome’s Main-Thread Task Scheduler and Task Priorities

The browser’s main thread faces competing demands, including executing JavaScript, responding to user input, and updating the screen with layout and paint. Chrome’s main-thread scheduler uses multiple task queues with different priorities to ensure that high-priority work, like handling input and animation, isn’t starved by low-priority tasks. This system is often referred to as the Blink scheduler, since Blink is Chrome’s rendering engine.

Chrome categorizes tasks internally. While the exact details are proprietary, generally, user input and event-handling tasks go into a high-priority queue, so that the scheduler will run them before other tasks if possible. Animation and rendering tasks also get a high priority; for example, the tasks that run for `requestAnimationFrame` callbacks are prioritized to keep animations smooth. In Figure 5-2, we can see the spectrum of task priorities from high end to low end.

![](bbox=[0.14, 0.48, 0.86, 0.77])

Figure 5-2. Task priority spectrum

You can visualize task priorities on a spectrum: at the high end are user-blocking tasks (input handlers, animations), in the medium range are normal operations (`setTimeout`, network callbacks), and at the low end are