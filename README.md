Kentico Custom Module Build Automation

Kentico CMS allows us to build custom functionality using custom modules. We can define module level settings (configs) as part of module definition along with default values. 

Kentico allows developers to create installation (nuget) packages for such custom modules and install it on other instances of Kentico.  Ater installing the custom modules on a Kentico instance you can override the module config values with site specific values.

Issues with the above approach:

1) We have to use Kentico module management interface to create installation packages for custom modules - which means its difficult to integrate with Continous Build tools like Octopus/Team City.

2) Kentico depends on the (development) database entries to create a module installation package - which is error prone as you can't assume the correctness of the development database.

3) We couldn't provide site specific config values for other Kentico instances.

Solution:
1) Introduce a custom Module.xml file per custom module - which holds the Webpart, FormControls, Config definitions with default values and site specific config values.

2) Create and AbstractModule class which in 'oninit' event reads the above mentioned Module.xml file and does the following:
  a) Create/Update WebParts
  b) Create/Update FormControls
  c) Create/Update Module Settings (configs)

3) Make new custom module inherit from the 'AbstractModule'.

4) Create MsBuild script to nuget package the module files (WebParts, FormControls, Pages and Module.xml)
