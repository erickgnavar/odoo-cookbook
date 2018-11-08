---
title: "Vista personalizada de modelo existente"
date: 2018-11-08T17:02:00-05:00
---

## Problema

Necesitamos agregar una vista `tree` de empleados `hr.employee` que contenga una lista de campos que la vista por defecto de Odoo no contiene.

- Los campos que se necesitan son `name`, `work_email` y `marital`.
- La vista `tree` por defecto tiene mas campos de los necesarios.

## Solución

En este caso necesitamos agregar una nueva vista sin modificar la existente para eso debemos crear una serie de componentes:

- Un nuevo menú `ir.menu`.
- Una acción `ir.actions.act_window`.
- Una vista asociada a una acción `ir.actions.act_window.view`.

Vamos a crear una vista `tree` que tenga los campos que se requieren:

```xml
<record id="hr_employee_view_tree_inherit" model="ir.ui.view">
    <field name="name">hr.employee.view.tree.inherit</field>
    <field name="model">hr.employee</field>
    <field name="arch" type="xml">
        <tree>
            <field name="name"/>
            <field name="work_email"/>
            <field name="marital"/>
        </tree>
    </field>
</record>
```

Creamos un menú y una acción asociados al modelo empleado `hr.employee`.

```xml
<record id="hr_employee_action" model="ir.actions.act_window">
    <field name="name">Employee</field>
    <field name="res_model">hr.employee</field>
    <field name="view_type">form</field>
    <field name="view_mode">tree,form</field>
</record>
```

```xml
<menuitem
        id="menu_hr_employee" name="Custom employee list"
        action="hr_employee_action" parent="hr.menu_open_view_employee_list_my"/>
```

Ahora si probamos este código veremos que el menú sigue trabajando con el `tree` existente de Odoo. Necesitamos conectar el nuevo menú con el `tree` que hemos creado. Para eso usamos una vista asociada a una acción:

```xml
<record id="hr_employee_tree_action_view" model="ir.actions.act_window.view">
    <field name="act_window_id" ref="hr_employee_action"/>
    <field name="view_mode">tree</field>
    <field name="view_id" ref="hr_employee_view_tree_inherit"/>
</record>
```

Aqui se especifica que cuando la acción `hr_employee_action` necesite mostrar una vista `tree` debe usar la vista `hr_employee_view_tree_inherit`.

Del mismo modo se puede definir el comportamiento para otros tipos de vistas como `form`, `kanban`, etc.
