# 4. Basic annotation

This section illustrates the use of HED tags and discusses various tags that are used to document
the structure and organization of electrophysiological experiments. The simplest annotations
treat each event as happening at a single point in time. The annotation procedure for such
events involves describing what happened during that event.

This chapter illustrates basic HED descriptions of four types of events that are often annotated
using single event markers: **stimulus events**, **response events**, 
**experiment control events**, and **data features**. 

HED also allows more sophisticated models of events that unfold over time using
multiple event markers. Downstream analyses often look for neurological effects directly 
following (or preceding) event markers. The addition of HED context, allows information 
about events that occur over extended periods of time to propagate to intermediate time points. 
[**Chapter 5: Advanced annotation**](05_Advanced_annotation.md#5-advanced-annotation) 
develops the HED concepts needed to capture these advanced models of events as well as event
and task inter-relationships. 


## 4.1. Instantaneous events

This section describes HED annotation of events that are modeled as happening at an instant in time. 
Sometimes the event marker corresponding to such an event is inserted in the data or held in an 
external event file containing the onset time of some action, relative to the beginning of the 
data recording. We refer to these events as **time-marked events**.
The event marker may also point to the end/offset of some happening or to time
between the onset and offset (for example, the maximum velocity point in a participant arm
movement or the maximum potential peak of an eye-blink artifact).

A typical example of an experiment using time-marked event annotation is simple target
in which geometric shapes of different colors are presented on a computer
screen at two-second intervals. After every visual shape presentation, the subject is asked to
press the left mouse button if the shape is a green triangle or the right mouse button otherwise.
After a block of 30 such presentation-response sequences (trials), the control software sounds a
buzzer to indicate that the subject can rest for 2 minutes before continuing to the next block of
trials. After the experiment is completed, the experiment runs an eyeblink-detection tool on the
EEG data and inserts an event marker at the amplitude maximum of each detected blink artifact.


## 4.2. Sensory presentations

The target detection experiment described above is an example of a stimulus-response paradigm:
perceptually distinct sensory stimuli are presented at precisely recorded times (typically with
abrupt onsets) and ensuing and/or preceding precisely-timed changes in the behavioral and
physiological data streams are annotated or analyzed. 
Stimulus onsets (typically) are annotated with the `Sensory-event` tag. 
Additional tags indicate task role. 
Separation of what an event is (as designated by a tag from the `Event` subtree) 
from its task role (as indicated by other descriptive tags) is an important design change
that distinguishes HED-3G from earlier versions
of HED and enables effective annotation in more complex situations.

A stimulus event can be annotated at different levels of detail. 
When not needed, fine details can generally be ignored, but once annotated can provide valuable information for later, 
possibly unanticipated analysis purposes. 
In a series of examples, we will annotate successively
more details about the experiment events. 
Each example shows both the short form and long form.
The elements in the long form that correspond to the short form are shown in bold-face. 
In addition, the long form includes a `Description` tag,
which is omitted from the short-form for readability. 

The following example illustrates a very basic annotation of a stimulus event, indicating 
the stimulus is a green triangle presented visually. The annotation states that 
this is a visual sensory event intended to be an experiment stimulus. 
`Sensory-event` is in the `Event` rooted tree and indicates the general class 
that this event falls into.

````{admonition} **Example:** Version 1 of a visual stimulus annotation.

**Short form:**  
> *Sensory-event, Experimental-stimulus, Visual-presentation, (Green, Triangle)*

**Long form:**  
> *Event/<strong>Sensory-event</strong>,*   
> *Property/task-property/Task-event-role/<strong>Experimental-stimulus</strong>,*  
> *Property/Sensory-property/<strong>Visual-presentation</strong>,*  
> *(Property/Sensory-property/Sensory-attribute/Visual-attribute/Color/CSS-color/Green-color/<strong>Green</strong>,*    
> *Item/Object/Geometric-object/2D-shape/<strong>Triangle</strong>),*  
> *Property/Informational-property/Description/An experimental stimulus consisting of*   
> *a green triangle is displayed on the center of the screen.*   

````

The example HED string above illustrates the most basic form of point event annotation. 
In general, the annotation for each event should include at least one tag from the 
`Event` tree. 
If there are multiple sensory presentations in the same event,
a single `Sensory-event` tag covers the general category for all presentations in the event.
The individual presentations (which may include different
modalities) are grouped with their descriptive tags,
while the `Sensory-event` tag applies overall. 
In this case there is only one, so the grouping is not necessary. 

The `Experimental-stimulus` is a `Task-property` tag.
Whether a particular sensory event is an experiment stimulus depends on the particular task,
hence `Experimental-stimulus` is a `Task-property`.
Sensory events that are extraneous to the task can also occur, 
so it is important to distinguish those that are related to the intent of the task.

The remaining portion of the annotation describes what the sensory presentation is. 
The `Green` and `Triangle` tags are grouped to indicate specifically that a green 
triangle is presented. 
`Visual-presentation` is a `Sensory-property` tag from the 
`Property` rooted tree. 
The senses are impacted by the `Sensory-event` should always be indicated, 
even if it appears to be obvious to the reader. 
The goal is to facilitate machine-actionable analysis.

HED has a number of qualitative relational tags designating spatial features such as 
`Center-of`, which should always be included if possible. These qualitative terms 
provide clear search anchors for tools looking for general positional characteristics.
Hemispheric and vertical distinctions have particular neurological significance. 
More detailed size, shape, and position information enhances the annotation. 
However, actual detailed information requires the specification of a frame 
of reference, a topic not addressed by the current HED specification.

**The order of the tags does not matter.**
HED strings are unordered lists of HED tags and tag groups. 
Where the grouping of associated tags needs to be indicated,
most commonly in the case of tags with modifiers, 
the related tags should be put in a tag group enclosed by parentheses (as above).  

Notice that the long form version also includes a `Description` tag that gives a text 
description of the event.
The `Description` tag is omitted for readability in the short form examples.
As a matter of practice, however, users should start with a detailed text description of each
type of event before starting the annotation. 
This description can serve as a check on the
consistency and completeness of the annotation. 
Generally users annotate using the short form 
for HED tags and use tools to map the short form into the long form during validation or analysis.


## 4.3. Task role

In deciding what additional information should be included, the annotator should 
consider how to convey the nature and intent of the experiment and the EEG responses 
that are likely to be elicited. The brief description suggests that green triangles 
are something “looked for”, within the structure of the task that participants are 
asked to perform during the experiment. The following annotation of the green triangle
presentation includes information about the role this stimulus appears in the task.

````{admonition} **Example:** Version 2 of a visual stimulus annotation.

**Short form:**  
> *Sensory-event, Experimental-stimulus, Visual-presentation,*  
> *(Green, Triangle), (Intended-effect, Oddball), (Intended-effect, Target)*  

**Long form:**  
> *Event/<strong>Sensory-event</strong>,*   
> *Property/Task-property/Task-event-role/<strong>Experimental-stimulus</strong>,*  
> *Property/Sensory-property/Sensory-presentation/<strong>Visual-presentation</strong>,*  
> *(Property/Sensory-property/Sensory-attribute/Visual-attribute/Color/CSS-color/Green-color/<strong>Green</strong>,*  
> *Item/Object/Geometric-object/2D-shape/<strong>Triangle</strong>),*  
> *(Property/Task-property/Task-effect-evidence/<strong>Intended-effect</strong>,*  
> *Property/Task-property/Task-stimulus-role/<strong>Oddball</strong>),*  
> *(Property/Task-property/Task-effect-evidence/<strong>Intended-effect</strong>,*  
> *Property/Task-property/Task-stimulus-role/<strong>Target</strong>),*  
> *Property/Informational-property/Description/A green triangle target oddball is presented*  
> *in the center of the screen with probability 0.1.*

````

The `Intended-effect` tag is a `Task-effect-evidence` tag that describes the effect
expected to be elicited from the participant experiencing the stimulus. 
This tag indicates, that based on the specification of the task, we can conclude
that the subject will be looking for the triangle (`Target`) and that its appearance
is unusual (`Oddball`). 

Three other tags in the `Task-effect-evidence` are `Computational-evidence`, 
`External-evidence`, and `Behavioral-evidence`. 
In many experiments, a subject indicates that something occurs by performing 
an action such as pushing the left mouse button for 
a green triangle and the right button otherwise. 
When the left-mouse button is pushed, 
one may conclude that the participant has behaved as though the green triangle appears. 
If the button push is tagged with `Behavioral-evidence`, automated tools can check whether 
the intended effect agrees with subject behavior. An example of `External-evidence` 
is annotation by a speech therapist about whether the participant stuttered in a speech
experiment. `Computational-evidence` might be generated from BCI annotation.

HED-3G has more sophisticated methods of specifying the relationships of events and tasks. 
These require more advanced tagging mechanisms that are discussed later in this document.


## 4.4. Agent actions

In many experiments, the participant is asked to press (or select and press) a finger button 
to indicate their perception of or judgment concerning the stimulus. These types of events, 
as well as participant actions not related to the task, are annotated as `Agent-action` events.
`Agent-action` events can be annotated with varying levels of detail, as illustrated by 
the next two examples.

````{admonition} **Example:** Version 1 of button press annotation.

**Short form:**  
 > *Agent-action, (Participant-response, (Press, Mouse-button))*

````

The `Participant-response` tag indicates that this event represents a task-related response 
to a stimulus. The `Press` tag is from the `Action` subtree and is grouped with the 
*Mouse-button* to indicate the pressing of a button. 
In general, `Action` elements can be considered verbs, 
while `Item` and `Agent` elements can be considered nouns. 
These elements form a natural sentence structure: (subject, (verb, direct object)), 
with the subject and direct object being formed by noun elements. `Property` 
elements are the adjectives, adverbs, and prepositions that modify and connect these elements.


````{admonition} **Example:** Version 2 of a button press annotation.

**Short form:**   
> *Agent-action, Participant-response,*   
> *((Human-agent, Experiment-participant), (Press, Mouse-button)),*  
> *(Behavioral-evidence, Oddball), (Behavioral-evidence, Target)*

**Long form:**  
> *Event/<strong>Agent-action</strong>,*  
> *Property/Task-property/Task-event-role/<strong>Participant-response</strong>,*    
> *((Agent/<strong>Human-agent</strong>,*    
> *Property/Agent-property/Agent-task-role/<strong>Experiment-participant</strong>),*   
> *(Action/Move/Move-body-part/Move-upper-extremity/<strong>Press</strong>,*   
> *Item/Object/Man-made-object/Device/IO-Device/Input-device/Computer-mouse/<strong>Mouse-button</strong>)),*   
> *(Property/Task-property/Task-effect-evidence/<strong>Behavioral-evidence</strong>,*   
> *Property/Task-property/Task-stimulus-role/<strong>Oddball</strong>),*   
> *(Property/Task-property/Task-effect-evidence/<strong>Behavioral-evidence</strong>,*   
> *Property/Task-property/Task-stimulus-role/<strong>Target</strong>),*   
> *Property/Informational-property/Description/The subject pushes the left mouse button*   
> *to indicate the appearance of an oddball target using index finger on the left hand.*   

````

The `Participant-response` tag is modified by tags that indicate that the participant is 
reacting by responding as though the stimulus were an oddball target. 
Specifically the `Behavioral-evidence` tag documents that the subject gave a response indicating an oddball target. 
In other words, the participant pressed the left mouse button indicating an oddball
target, which may or may not match the stimulus that was presented. 

Other details should be annotated, including whether the subject’s left, right, 
or dominant hand was used to press the mouse button and whether the left mouse button 
or right mouse button was pressed. (This factor was indicated in the description,
but not in the machine-actionable tags.)


## 4.5. Experimental control

Experiments may have experiment control events written into the event record, 
often automatically by the presentation or control software. 
In the illustration provided above, 
a buzzer sounded by the control software indicates that the subject should rest.

````{admonition} **Example:** Version 1 of a simple feedback event.

**Short form:**  
> *Sensory-event, Instructional, Auditory-presentation,*  
> *(Buzz, (Intended-effect, Rest))*  

**Long form:**  
> *Event/<strong>Sensory-event</strong>,*   
> *Property/Task-property/Task-event-role/<strong>Instructional</strong>,*  
> *Property/Sensory-property/Sensory-presentation/<strong>Auditory-presentation</strong>,*  
> *(Item/Sound/Named-object-sound/<strong>Buzz</strong>,*  
> *(Property/Task-property/Task-effect-evidence/<strong>Intended-effect</strong>,*   
> *Action/Perform/<strong>Rest</strong>)),*  
> *Property/Informational-property/Description/A buzzer sounds indicating a rest period.*  

````

## 4.6. Data features

Another type of tagging documents computed data features and expert annotations 
that have been inserted post-hoc into the experimental record as events. 
The `Computed-feature` and `Observation` tags designate whether the event came 
from a computation or from manual evaluation.
The following example illustrates a HED annotation computed from a program.

````{admonition} **Example:** Annotation of an inserted computed feature.

**Short form:**  
> *Data-feature, (Computed-feature, Label/Blinker_BlinkMax)*  

**Long form:**  
> *Event/<strong>Data-feature</strong>,*   
> *(Property/Data-property/Data-source-type/<strong>Computed-feature</strong>,*     
> *Property/Informational-property/<strong>Label/Blinker_BlinkMax</strong>),*   
> *Property/Informational-property/Description/Event marking the maximum signal*    
> *deviation caused by blink inserted by the Blinker tool.*   

````

As shown by this example, the `Computed-feature` tag is grouped with a label of the form
`toolName_featureName`, in this case the Blinker tool for detecting eye-blinks in EEG.
The computed property is just a marker of where a feature was detected. 
If a value was computed at this point, an additional *Data-value* tag would be included.

Clinical evaluations are observational features,
and many fields have standardized names for these features.
Although the HED standard itself does not specify these names, 
library schema representing terminology in clinical or application subfields may provide 
the vocabulary. 
[**Chapter 7: Library schemas**](./07_Library_schemas.md) presents some rules for schema developers.

The following example illustrates how annotation from a human expert can be annotated in HED.

````{admonition} **Example:** Annotator AJM identifies a K-complex in a sleep record.

**Short form:**   
> *Data-feature, (Observation, Label/AnnotatorAJM_K-complex)*

**Long form:**  
> *Event/<strong>Data-feature</strong>,*  
> *(Property/Data-property/Data-source-type/<strong>Observation</strong>,*  
> *Property/Informational-property/<strong>Label/AnnotatorAJM_K-complex</strong>),*  
> *Property/Informational-property/Description/K-complex defined by AASM guide.*  

````

## 4.7. What else?

Most event annotation focuses on basic identification and description of stimuli and the
participant’s direct response to that stimuli. 
However, for accurate comparisons across studies,
much more information is required and should be documented with HED tags 
rather than just with text descriptions.
This is particularly true if this information is relevant to the experimental intent,
varied during the experiment, or likely to evoke a neural response. 

The example of [**Chapter 4.1:Instantaneous events**](#41-instantaneous-events), 
models the sensory presentation of the stimulus images happening at a single point in time.
More realistically, the green triangle might be displayed 
for an extended period (during which other events might occur). 
Further, the disappearance of the triangle is likely to elicit a neural response.
Exactly how this information should be represented is discussed in 
[**Chapter 5.3: Temporal scope**](05_Advanced_annotation.md#53-temporal-scope).

Even for a standard setup, aspects such as the screen size,
the distance and position of the participant relative to the screen and the stimulus,
as well as other details of the environment,
should be documented as part of the overall experiment context. 
These details allow analysis tools to compare and contrast studies or to 
translate visual stimuli into visual field information. 
`Event-context` tags, which are introduced in 
[**Chapter 5.5: Event contexts**](05_Advanced_annotation.md#55-event-contexts), 
allow this information to be propagated to recording events in a manner 
that is convenient for analysis.

HED also allows the embedding of annotations for the design of the experiment,
documenting how and when condition variables and other aspects of an experiment are changed.  
[**Chapter 5.6: Experimental design**](05_Advanced_annotation.md#56-experimental-design) 
describes HED mechanisms for annotating this information.
