# OGC.Engineering
## ogc-lib-os-nrtps - Non Real-Time Polled Scheduler ( NRTPS )
Developer contact - dustin < at > ogc.engineering

---
- small footprint
    - Core logic      ( 438 Bytes )
    - Per Event Slot  ( 14 Bytes each ) /* adjustable depending on task count */
- cooperative
    - Tasks MUST yield for other tasks to be able to operate

Library provides main function with general system initialization calls and the nrtps loop call that scans a task array
checking for events that are ready.

This is a VERY cooperative system passing full system control over the each task.  It is up to each task to do the
smallest chunk of processing possible before yielding control so the scheduler can check for other tasks that are ready.

---
Requirements:

---

- the DEPLOYMENT/PROJECT layer(folder) must contain:
    - app.h header that redirects to the primary application ( because multiple applications are possible )

- the APP layer(folder) must define:
    - void app_init( void );
        - The function app_init() must schedule initial tasks to start operations
        - Task rescheduling can occur in serveal ways:
            - Tasks can schedule themselves or other tasks via a yield or timed ogc-lib-os-nrtps-set() call
            - Timer interrupts can schedule a task at regular intervals in the same manner

- the HAL layer(folder) must define:
    - void hal_config ( void );
        - configuration of the watchdog, clocks, pins, and other things that do not require the scheduler
    - void hal_init( void );
        - initialization of hal level nrtps tasks that need the scheduler ( i.e. background tasks )

---
Optional 
---
- define OGC_LIB_OS_NRTPS__EVENT_ARRAY_SIZE in the deployment header to override the default task array size
- define DEPLOYMENT_OPTION_RUN_UNIT_TESTS to test the library and other supported units
