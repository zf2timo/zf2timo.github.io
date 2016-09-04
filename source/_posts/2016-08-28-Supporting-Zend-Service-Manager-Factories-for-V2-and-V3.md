---
title: Supporting Zend Service Manager Factories for V2 and V3
tags: [zend, factory]
categories: [development]

---

When creating a module it can be useful to support Zend ServiceManager Version V2 and V3.
I faced the situation, because my Module had to work in an Application based on Zend 2.4 and another on Zend 3.0. So if we
look at the `FactoryInterface` in both Version, we easily see the Problem:

~~~php
namespace Zend\ServiceManager\Factory;

use Interop\Container\ContainerInterface;
use Interop\Container\Exception\ContainerException;
use Zend\ServiceManager\Exception\ServiceNotCreatedException;
use Zend\ServiceManager\Exception\ServiceNotFoundException;

interface FactoryInterface
{
    public function __invoke(ContainerInterface $container, $requestedName, array $options = null);
}
~~~

~~~php
namespace Zend\ServiceManager;

interface FactoryInterface
{
    public function createService(ServiceLocatorInterface $serviceLocator);
}
~~~
As you can see, the Interface was moved into another Namespace - which was the right decision. But as a Module creator,
you can't implement both Interfaces. Of course, one of the Versions will always be missing and it leads into a PHP Fatal Error.

To solve the Problem just implement non of the `FactoryInterface`'s and manually create the Methods like in this example:
~~~php
namespace Foo;

class FooFactory
{
    // support for Zend Service Manager V3
    public function __invoke($container, $requestedName, array $options = null)
    {
        return new Foo($container->get(SomeDependency::class));
    }
    
    // support for Zend Service Manager V2
    public function createService($serviceLocator)
    {
        return $this->__invoke($serviceLocator, 'Foo\Foo');    
    }
}
~~~
Please notice, i also removed the type Hinting from the Methods. This allows us to pass the ServiceManger V2 into the
`__invoke` Method.

## Why dose it works

This is really simple. The Service Manager checks in both Versions if the instantiated Factory supports the required Method.
If the Method exists for the installed Version, it assumes, the Method is the factory method and calls it.
