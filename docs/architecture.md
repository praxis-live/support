# Architecture & terminology

## A forest not a tree

_Praxis LIVE_ has an architecture influenced by message-passing models for distributed / concurrent programming. It is the strict adherence to this architecture that is a key strength, allowing for working with multiple media without interference (or multiple pipelines of the same media such as video at different frame rates). It also provides the ability to background load resources, process data, etc. without interfering with playback. And it allows for projects to run transparently across multiple processes (for better performance), or even across multiple networked devices.

Individual components within _Praxis LIVE_ (such as a video effect, sample player, button, etc.) exist within a tree of components, which can be multiple levels deep. All components have an address, which follows a familiar slash-separated syntax (eg. `/audio/delay1`). The first level of this tree is a special type of component called a **Root**. Roots generally provide the media context for the components that exist within them, and there are currently 6 types of root available within the standard _Praxis LIVE_ install – audio, video, TinkerForge, OSC, MIDI and GUI. While you would commonly have one of each type you require, it is perfectly possible to have as many roots of the same type as your system(s) can handle.

All roots exist within the **Root Hub**. The root hub is a container for roots, but it is not itself a component. All roots are sand-boxed from each other – there is no way for any root or component within it to directly access another root or its components. Instead, the hub acts as a router to pass messages between different roots, local or remote.

**@TODO Root Hub image**

Here you can see the **Hub Manager** within _Praxis LIVE_, which gives you a visual representation of the root hub. You will notice that there are 3 user roots running (audio, video and GUI), but that the button is toggled to also show you the system roots. This is another key part of the architecture – all of the system code is equally sand-boxed and confined to using the message-passing system.

## Ports & controls

Components within _Praxis LIVE_ have two ways of communicating – **Ports** and **Controls**. Ports are used for instantaneous communication with sibling components (share the same parent). When you connect components by drawing lines in the graph editor, you are connecting ports together. They are a lightweight mechanism for sharing control data, video frames, audio buffers, etc. Although you are unlikely to need to know this, ports also have an address (eg. `/audio/delay!in`).

Controls are the basis of _Praxis LIVE’s_ message-passing system. They receive, react and respond to messages usually coming from a component within another root. All communication _without exception_ between components in different roots ends with a control receiving a message. Controls have an address consisting of their component address and ID (eg. `/audio/delay.time`) – the dot syntax is a deliberate parallel with method calls.

**@TODO Ports and Controls image**

Above you can see an example of an `audio:sampleplayer` component in the editor. Ports can be seen on the graph component and controls within the component editor window – note that many controls will have a corresponding port, and vice versa.

When you manipulate values within the dialog, you are sending messages to controls on the component. There are three different types of controls – actions, properties and functions. Actions are represented by the buttons at the top of the dialog, and properties within the table. Functions are not currently reflected within the editor, and are primarily used internally.

The `ready` and `error` ports reflect the fact that resources are loaded in the background. When the `sample` port receives a file address, a request is passed to one of the system roots that handles background loading of resources. Once the request is completed then a message is sent from the ready or error port depending on whether the sample loaded successfully. Other components that load resources use the same methodology.

## Further information

An old but detailed explanation of the underlying _Praxis LIVE_ architecture is available on the development blog -  [The influence of the actor model](http://praxisintermedia.wordpress.com/2012/07/26/the-influence-of-the-actor-model/)
