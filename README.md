Download Link: https://assignmentchef.com/product/solved-cs311-assignment-5
<br>
Upgrade the simulator to a discrete event simulator.

<h1>Inputs and Outputs</h1>

The inputs and outputs stay the same. The inputs are the configuration file, the path where the statistics file is to be created, and the object file. The output is the statistics file. Report throughput in terms of instructions per cycle in the statistics file.

<h1>New Source Files</h1>

Place the new source files: Element.java, Event.java, EventQueue.java, ExecutionCompleteEvent.java, MemoryReadEvent.java, MemoryResponseEvent.java, MemoryWriteEvent.java in the generic package.

<h2><strong>Updation of </strong>Simulator.java</h2>

<ul>

 <li>Add the following data member to the class: static EventQueue eventQueue;.</li>

 <li>Add the following line to the constructor: eventQueue = new EventQueue();.</li>

 <li>Update the loop in simulate() to look like this:</li>

</ul>

while ( not end of          simulation )

{ performRW performMA performEX eventQueue . processEvents (); performOF performIF

increment        clock by 1

}

<ul>

 <li>Add this function to the class:</li>

</ul>

public             static EventQueue getEventQueue ()

{ return eventQueue ;

}

<h1>Discrete Event Simulator Model</h1>

An event is a tuple of the form: <em>&lt;</em>event time, event type, requesting element, processing element, payload<em>&gt;</em>. The event queue is a list of events ordered by time. An event is said to “<em>fire</em>” when the current clock cycle is equal to the event time. When this happens, the handleEvent() function of the processing element is invoked. Handling of an event may in turn lead to more events being generated, for the same clock cycle, or for some future clock cycle.

Decide which units you wish to work using events, and which directly through function calls from the main loop. For all units that you believe will recieve events, make them implement the Element interface. This will then require you to implement a handleEvent() function for that unit.

You may create more Event classes, or modify the existing ones.

<strong>Example</strong>

Below is a brief illustration of the Instruction Fetch stage. It is by no means complete. It is only to give you the basic idea.

public           void performIF ()

{ i f ( IF EnableLatch . isIF enable ())

{ i f ( IF EnableLatch . isIF busy ())

{ return ;

}

Simulator . getEventQueue (). addEvent( new MemoryReadEvent(

Clock . getCurrentTime () + Configuration . mainMemoryLatency , this , containingProcessor . getMainMemory() ,

containingProcessor . getRegisterFile (). getProgramCounter ( )) );

IF EnableLatch . setIF busy ( true );

}

}

@Override public void handleEvent (Event e) { i f (IF OF Latch . isOF busy ())

{

e . setEventTime(Clock . getCurrentTime () + 1);

Simulator . getEventQueue (). addEvent(e );

} else

{

MemoryResponseEvent event = (MemoryResponseEvent) e ;

IF OF Latch . setInstruction ( event . getValue ());

IF OF Latch . setOF enable ( true );

IF EnableLatch . setIF busy ( false );

}

}

Below is the code snippet from the MainMemory.java class.

@Override public             void handleEvent (Event e) { i f (e . getEventType () == EventType .MemoryRead)

{

MemoryReadEvent event = (MemoryReadEvent) e ;

Simulator . getEventQueue (). addEvent( new MemoryResponseEvent( Clock . getCurrentTime () , this , event . getRequestingElement () , getWord( event . getAddressToReadFrom ( ) ) ) ) ; }

}

<h1>Functionalities to be Implemented</h1>

<ul>

 <li>Modeling the latency of the main memory</li>

</ul>

<strong>– </strong>This should reflect in both instruction fetches and load/ store operations

<ul>

 <li>Modeling the latencies of different functional units: ALU, multiplier, divider, etc.</li>

</ul>