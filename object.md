# Object of the specification

The Kairos project is a software system that allow consumers to take an appointment with the software administrators. By this way the software must offer to the consumers an human to machine interface that will permit to perform an appointment registering or a time slot reservation.

In order to achieve this objective, the software is divided by many parts :
 * __chronos__ part : an effective public API that allow the consumers to get the available slots, perform search requests, and offer administration panel to the administrators.	
 * __hermes__ part : a socket based double way interaction application that send notifications to the consumers and receive fast applyence orders.
 * __worker__ part : a system application to process slow working task.

## Administrator user

An administrator user is an entity that want to be abble to receive appointments with his clients. It has to abbility to connect to the application and see it's profile and appointments.