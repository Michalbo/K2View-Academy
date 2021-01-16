# TDM LU Implementation - Generic Guidelines

The TDM task copies a selected [Business Entity](/articles/TDM/tdm_overview/03_business_entity_overview.md) from the selected source environment to the selected target environment. A Business Entity can have multiple [LUs](/articles/TDM/tdm_overview/(/articles/03_logical_units/01_LU_overview.md)) with a flat or a hierarchical structure. For example, a Customer Business Entity can consist of Customer Care, Billing, Ordering and Usage LUs. The ability to break a BE up into several LUs enables maximum flexibility and avoiding duplicate development. In addition, defining a hierarchical structure of parent-child LUs enables creating LUs based on the natural root entity of the related data sources instead of forcefully setting unified root entities on all LUs related to a given BE.

Each LU in the TDM project must have additional components to support TDM functionality.

Copy the following objects from the [TDM_LIBRARY LU](/articles/TDM/tdm_implementation/04_fabric_tdm_library.md#tdm_library-lu) into each LU:

<table width="900pxl">
<thead>
<tr>
<td valign="top" width="200pxl">
<p><strong>Object Name</strong></p>
</td>
<td valign="top" width="250pxl">
<p><strong>Object Description</strong></p>
</td>
<td valign="top" width="450pxl">
<p><strong>Implementation Instructions</strong></p>
</td>
</tr>
</thead>
<tbody>
<tr>
<td valign="top" width="200pxl">
<p><h4>Globals</p>
</td>
<td valign="top" width="250pxl">
<p>TDM LU level globals.</p>
</td>
<td valign="top" width="450pxl">
<p>Populate the ROOT_TABLE_NAME using the main source table or tables. Several source tables can be populated when separated by a comma. For example: CUSTOMER, ACCOUNT.</p>
  <p>The <strong>fnCheckInsFound</strong> enrichment function (attached to the root LU table) validates the source data and verifies that the entity (IID) exists in the main source tables. If the entity is not found in the main source tables, this function throws an Exception.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p><h4>trnChildLink</p>
</td>
<td valign="top" width="250pxl">
<p>Translation for mapping parent and child IDs.</p>
</td>
<td valign="top" width="450pxl">
<p>This translation must be added and populated on each <strong>parent LU</strong>. The child_lu field must be populated by the name of the child LU.</p>
<p>The child_lu_eid_sql field must be populated by the SQL which runs on the parent LU and gets the child IDs for each parent ID.</p>
<p><strong>Example:</strong><u><br /></u>Customer LU is the parent of the Order LU. <br />trnChildLink of the Customer LU must be populated as follows:</p>
<ul>
<li><strong>Child_lu = </strong>Customer</li>
<li><strong>Child_lu_eid_sql = </strong>select order_id from subscriber</li>
</ul>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p><h4>trnLuParams</p>
</td>
<td valign="top" width="250pxl">
<p>Translation for the population of the LU_PARAMS table.</p>
</td>
<td valign="top" width="450pxl">
<p>This translation must be added and populated for each LU to enable a selection of entities by predefined parameters (for example- copy business customers).<br />COLUMN_NAME is populated by the name of the parameter and the SQL is populated by the SQL query which gets the values for the defined parameter.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p><h4>TDM_LU_TYPE_RELATION_EID</p>
</td>
<td valign="top" width="250pxl">
<p>Relation table.</p>
</td>
<td valign="top" width="450pxl">
<p>This table must be added to each LU.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p><h4>FABRIC_TDM_ROOT</p>
</td>
<td valign="top" width="250pxl">
<p>Root table of each LU. Contains the LUI, source environment, IID, task_name (version_name), and timestamp (version_datetime). The version_name and version_datetime are populated on <a href="/articles/TDM/tdm_overview/02_tdm_glossary.md#data-flux">DataFlux tasks</a>.</p>
</td>
<td valign="top" width="450pxl">
<p>This table must be added to each LU. Verify that the fnCheckInsFound enrichment function under Shared Objects is attached to this LU table.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p><h4>INSTANCE_TABLE_COUNT</p>
</td>
<td valign="top" width="250pxl">
<p>Statistics table. This table is used for the [TDM Statistics report] of each TDM task execution.</p>
</td>
<td valign="top" width="450pxl">
<p>This table must be added to each LU.</p>
</td>
</tr>
<tr>
<td valign="top" width="200pxl">
<p><h4>LU_PARAMS</p>
</td>
<td valign="top" width="250pxl">
<p>Parameters table.</p>
</td>
<td valign="top" width="450pxl">
<p>This table must be added to each LU. Each parameter populated in the trnLuParams must be added as a column to the LU_PARAMS table.</p>
</td>
</tr>
</tbody>
</table>


- Add the **FABRIC_TDM_ROOT** LU table to the LU Schema and set it as a [Root table](/articles/03_logical_units/08_define_root_table_and_instance_ID_LU_schema.md). Set the **Instance PK Column** to **k2_tdm_eid** column.

- Add the **LU_PARAMS**, **INSTANCE_TABLE_COUNT** and **TDM_LU_TYPE_RELATION_EID** to the LU Schema. Connect these tables to the Entity ID column of the main source LU table. For example, the main source LU table of Customer LU is Customer. CUSTOMER.CUSTOMER_ID is linked to the FABRIC_ROOT_TABLE.IID column. Link the LU_PARAMS, INSTANCE_TABLE_COUNT  and TDM_LU_TYPE_RELATION_EID to the CUSTOMER.CUSTOMER_ID column:

  ![tdm lu example](images/tdm_lu_example1.png)

  

- The LU_PARAMS LU table must be added to each LU Schema, even when it is not required for defining parameters on the LU. In this case, the LU_PARAM table only contains the ENTITY_ID and SOURCE_ENVIRONMENT fields.

- Edit the trnLuParams and LU_PARAMS to add selection parameters to the LU. 

  Click for more information about [Handling TDM Parameters](07_tdm_implementation_parameters_handling.md).

- Sensitive data on LU tables must be masked using a Broadway population and the [masking actor](/articles/19_Broadway/actors/07_masking_and_sequence_actors.md). 

 Click for more information about [TDM Masking].

### LU Debug

The LUI must include the source environment which must be set as the [active environment](/articles/25_environments/01_environments_overview.md) of Fabric. When debugging the TDM implementation on Fabric debug server, either:

- Populate the source environment using `_dev_`. For example, **_dev_1**.
- Create and deploy the environment to the Fabric debug server, set the source environment as an active environment and concatenate this source environment to the Entity ID (IID). For example, **UAT_1**.  

[![Previous](/articles/images/Previous.png)](04_fabric_tdm_library.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](06_tdm_implementation_support_hierarchy.md)
