## Stages Management

The Stages comprising a Flow are shown in the bar below the Flow header: 

 ![image](/articles/DPM/images/Figure_11_Flow_Stages.png)

- Use the + buttons to add more Stages
- Use the x buttons to delete a Stage

The duration of each Stage is marked in the yellow tag at the top of the Stage arrow. This duration is based on the SLAs of the tasks within the Stage and also takes into consideration their order and dependencies. 
The Stage name and description can be changed as long as the Flow is not marked as Completed. Edit this information by using the   ![image](/articles/DPM/images/Figure_11a_edit_stage_icon.png) button, located at the right side of the Stage header section. 

### Add/Edit a Task

A Stage is composed of one or more Tasks. Each Task performs a specific action in the Request Fulfilment process, for example: 

- Validate the customer request.
- Send an e-mail to the customer acknowledging the request was registered.
- Gather the required customer data.
- Review the gathered data.

To add a new Task under a specific Stage, select the Stage by clicking the Stage name on the Stage bar and use the  ![image](/articles/DPM/images/Figure_12a_new_task_icon.png) option that appears on the right side. 
The Task Configuration screen will be shown. This screen includes several tabs, each configuring a different aspect of the Task.

![image](/articles/DPM/images/Figure_12_Add_Edit_a_Task_screen.png)

The Task configuration includes the following tabs: Tasks, Reminders, Operations, and Associated Tasks. 

Tasks can be divided into two primary categories: Tasks that are designed to be executed manually, and Tasks that are configured to run automatically. An automatic Task is performed by the DPM system, while a manual Task will be allocated to the Stewards and their teams. 

The following sections describe the properties that should be configured when defining a Task, and the different execution configurations of automatic and manual tasks. 



[![Previous](/articles/DPM/images/Previous.png)](/articles/DPM/02_Admin_Module/04_Flows.md)[<img align="right" width="60" height="54" src="/articles/DPM/images/Next.png">](/articles/DPM/02_Admin_Module/05_Tasks.md)