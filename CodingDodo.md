## [Dodo](https://codingdodo.com/)
## Odoo JavaScript Tutorial 101 - Part 1: Classes and MVC architecture overview
1. The core web.Class of Odoo
   + web.Class
2. The web.Widget Odoo Class.
   + The web.Widget extends the PropertiesMixin
     + extends the EventDispatcherMixin
     + extends the ParentedMixin
   + The web.Widget extends the ServicesMixin
     + loadViews, do_action, get_session, _rpc
     + trigger_up
3. Let's recap this mixins mess, so what can the web.Widget do ?
   + To make a summary of the inheritance tree, the web.Widget has access to:
     + The ParentedMixin for getChildren, getParent, etc
     + The EventDispatcherMixin to handle events and trigger_up
     + The PropertiesMixin to get and set
     + The ServicesMixin to make RPC calls, call actions, load views, etc.
4. The actual content of the web.Widget class
   +  the web.Widget main purpose is to render itself with QWeb, do its life cycle management, and inserting itself into the DOM
5. Lifecycle
   + The Life cycle goes init -> willStart ->[rendering]-> start -> destroy
   + 組件使用，搭配hook比較容易抓到state的狀態
   + 
6. 
