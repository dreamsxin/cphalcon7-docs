Class **Phalcon\\Acl\\Adapter\\Memory**
=======================================

*extends* abstract class :doc:`Phalcon\\Acl\\Adapter <Phalcon_Acl_Adapter>`

*implements* :doc:`Phalcon\\Acl\\AdapterInterface <Phalcon_Acl_AdapterInterface>`, :doc:`Phalcon\\Di\\InjectionAwareInterface <Phalcon_Di_InjectionAwareInterface>`, :doc:`Phalcon\\Events\\EventsAwareInterface <Phalcon_Events_EventsAwareInterface>`

.. role:: raw-html(raw)
   :format: html

:raw-html:`<a href="https://github.com/dreamsxin/cphalcon7/blob/master/ext/acl/adapter/memory.c" class="btn btn-default btn-sm">Source on GitHub</a>`

Manages ACL lists in memory  

.. code-block:: php

    <?php

    $acl = new Phalcon\Acl\Adapter\Memory();
    
    $acl->setDefaultAction(Phalcon\Acl::DENY);
    
    //Register roles
    $roles = array(
    	'users' => new Phalcon\Acl\Role('Users'),
    	'guests' => new Phalcon\Acl\Role('Guests')
    );
    foreach ($roles as $role) {
    	$acl->addRole($role);
    }
    
    //Private area resources
      $privateResources = array(
    	'companies' => array('index', 'search', 'new', 'edit', 'save', 'create', 'delete'),
    	'products' => array('index', 'search', 'new', 'edit', 'save', 'create', 'delete'),
    	'invoices' => array('index', 'profile')
    );
    foreach ($privateResources as $resource => $actions) {
    	$acl->addResource(new Phalcon\Acl\Resource($resource), $actions);
    }
    
    //Public area resources
    $publicResources = array(
    	'index' => array('index'),
    	'about' => array('index'),
    	'session' => array('index', 'register', 'start', 'end'),
    	'contact' => array('index', 'send')
    );
      foreach ($publicResources as $resource => $actions) {
    	$acl->addResource(new Phalcon\Acl\Resource($resource), $actions);
    }
    
      //Grant access to public areas to both users and guests
    foreach ($roles as $role){
    	foreach ($publicResources as $resource => $actions) {
    		$acl->allow($role->getName(), $resource, '*');
    	}
    }
    
    //Grant access to private area to role Users
      foreach ($privateResources as $resource => $actions) {
     		foreach ($actions as $action) {
    		$acl->allow('Users', $resource, $action);
    	}
    }



Methods
-------

public  **__construct** ()

Phalcon\\Acl\\Adapter\\Memory constructor



public *boolean*  **addRole** (*Phalcon\\Acl\\RoleInterface|string* $role, [*array|string* $accessInherits])

Adds a role to the ACL list. Second parameter allows inheriting access data from other existing role Example: 

.. code-block:: php

    <?php

     	$acl->addRole(new Phalcon\Acl\Role('administrator'), 'consultant');
     	$acl->addRole('administrator', 'consultant');




public  **addInherit** (*string* $roleName, *string* $roleToInherit)

Do a role inherit from another existing role



public *boolean*  **isRole** (*string* $roleName)

Check whether role exist in the roles list



public *boolean*  **isResource** (*string* $resourceName)

Check whether resource exist in the resources list



public *boolean*  **addResource** (*Phalcon\\Acl\\Resource|string* $resource, [*array* $accessList])

Adds a resource to the ACL list Access names can be a particular action, by example search, update, delete, etc or a list of them Example: 

.. code-block:: php

    <?php

     //Add a resource to the the list allowing access to an action
     $acl->addResource(new Phalcon\Acl\Resource('customers'), 'search');
     $acl->addResource('customers', 'search');
    
     //Add a resource  with an access list
     $acl->addResource(new Phalcon\Acl\Resource('customers'), array('create', 'search'));
     $acl->addResource('customers', array('create', 'search'));




public  **addResourceAccess** (*string* $resourceName, *mixed* $accessList)

Adds access to resources



public  **dropResourceAccess** (*string* $resourceName, *mixed* $accessList)

Removes an access from a resource



protected  **_allowOrDeny** (*string* $roleName, *string* $resourceName, *string* $access, *string* $action, [*callable* $callback])

Checks if a role has access to a resource



public  **allow** (*string* $roleName, *string* $resourceName, *mixed* $access, [*callable* $callback])

Allow access to a role on a resource You can use '*' as wildcard Example: 

.. code-block:: php

    <?php

     //Allow access to guests to search on customers
     $acl->allow('guests', 'customers', 'search');
    
     //Allow access to guests to search or create on customers
     $acl->allow('guests', 'customers', array('search', 'create'));
    
     //Allow access to any role to browse on products
     $acl->allow('*', 'products', 'browse');
    
     //Allow access to any role to browse on any resource
     $acl->allow('*', '*', 'browse');




public *boolean*  **deny** (*string* $roleName, *string* $resourceName, *mixed* $access, [*callable* $callback])

Deny access to a role on a resource You can use '*' as wildcard Example: 

.. code-block:: php

    <?php

     //Deny access to guests to search on customers
     $acl->deny('guests', 'customers', 'search');
    
     //Deny access to guests to search or create on customers
     $acl->deny('guests', 'customers', array('search', 'create'));
    
     //Deny access to any role to browse on products
     $acl->deny('*', 'products', 'browse');
    
     //Deny access to any role to browse on any resource
     $acl->deny('*', '*', 'browse');




public *boolean*  **isAllowed** (*mixed* $role, *mixed* $resource, *string* $access, [*mixed* $data])

Check whether a role is allowed to access an action from a resource 

.. code-block:: php

    <?php

     //Does andres have access to the customers resource to create?
     $acl->isAllowed('andres', 'Products', 'create');
    
     //Do guests have access to any resource to edit?
     $acl->isAllowed('guests', '*', 'edit');




public :doc:`Phalcon\\Acl\\Role <Phalcon_Acl_Role>` [] **getRoles** ()

Return an array with every role registered in the list



public :doc:`Phalcon\\Acl\\Resource <Phalcon_Acl_Resource>` [] **getResources** ()

Return an array with every resource registered in the list



public *string[]*  **getAccess** ()

Return the access action



public  **setDefaultAction** (*int* $defaultAccess) inherited from Phalcon\\Acl\\Adapter

Sets the default access level (Phalcon\\Acl::ALLOW or Phalcon\\Acl::DENY)



public *int*  **getDefaultAction** () inherited from Phalcon\\Acl\\Adapter

Returns the default ACL access level



public *string*  **getActiveRole** () inherited from Phalcon\\Acl\\Adapter

Returns the role which the list is checking if it's allowed to certain resource/access



public *string*  **getActiveResource** () inherited from Phalcon\\Acl\\Adapter

Returns the resource which the list is checking if some role can access it



public *string*  **getActiveAccess** () inherited from Phalcon\\Acl\\Adapter

Returns the access which the list is checking if some role can access it



public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector) inherited from Phalcon\\Di\\Injectable

Sets the dependency injector



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** ([*unknown* $error], [*unknown* $notUseDefault]) inherited from Phalcon\\Di\\Injectable

Returns the internal dependency injector



public  **setEventsManager** (:doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>` $eventsManager) inherited from Phalcon\\Di\\Injectable

Sets the event manager



public :doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>`  **getEventsManager** () inherited from Phalcon\\Di\\Injectable

Returns the internal event manager



public *boolean*  **fireEvent** (*string* $eventName, [*mixed* $data], [*unknown* $cancelable]) inherited from Phalcon\\Di\\Injectable

Fires an event, implicitly calls behaviors and listeners in the events manager are notified



public *mixed*  **fireEventCancel** (*string* $eventName, [*mixed* $data], [*unknown* $cancelable]) inherited from Phalcon\\Di\\Injectable

Fires an event, can stop the event by returning to the false



public *boolean*  **hasService** (*string* $name) inherited from Phalcon\\Di\\Injectable

Check whether the DI contains a service by a name



public :doc:`Phalcon\\Di\\ServiceInterface <Phalcon_Di_ServiceInterface>`  **setService** (*unknown* $name) inherited from Phalcon\\Di\\Injectable

Sets a service from the DI



public *object|null*  **getService** (*unknown* $name) inherited from Phalcon\\Di\\Injectable

Obtains a service from the DI



public *mixed*  **getResolveService** (*string* $name, [*array* $args], [*unknown* $noerror], [*unknown* $noshared]) inherited from Phalcon\\Di\\Injectable

Resolves the service based on its configuration



public  **attachEvent** (*string* $eventType, *Closure* $callback) inherited from Phalcon\\Di\\Injectable

Attach a listener to the events



public  **__get** (*unknown* $property) inherited from Phalcon\\Di\\Injectable

Magic method __get



public  **__sleep** () inherited from Phalcon\\Di\\Injectable

...


public  **__debugInfo** () inherited from Phalcon\\Di\\Injectable

...


