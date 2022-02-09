


Path to file that contains trigger resource definition
With our Delivery pipeline wizard there is no need to manually create and define sensor dependencies for Argo Events.
CSDP brings in all the information you need 


### Delivery pipeline concepts
The name of the delivery pipeline is created from the na,e of the sensor and the name of the trigger to conform to Aro Event 

Git source
The Git source is the repository that houses the resources of the delivery pipeline. All commits to pipeline resources are made here. The Git source also determines which CSDP runtime executes the pipeline.



#### Workflow templates



#### Trigger conditions
Trigger Conditions collates all the requirements to set up Argo Events and define dependencies. Here, you select the event source, the event type,  arguments, and event filters, all in the same location. On commit, CSDP generates the event source and sensor manifests.

**Event sources**  
Currently we support GitHub and Cron event sources.

**Event types**  
Every event source has a set predefined event types you can select from. 

**Arguments**  
An event type has a set of arguments you can optionally configure for that event. The arguments are pulled in from the Workflow Template you selected for the delivery pipeline. Create argument definitions through variables or custom definitions.  

Every argument has a set of predefined variables you can select from. CSDP automatically maps the argument to the path in the manifest that is generated on commit. There is no need to remember or copy resource paths when you create pipelines as CSDP does it for you. Argo Workflows extracts the value from the event payload at runtime. 

**Filters**  
Filters in Trigger Conditions create dependencies for event triggers. Use filters to enforce validity constraints on event triggers. For example, add a filter to a Git push event that triggers it only on push to a particular _branch_.  

CSDP supports Argo Events Data filters.

