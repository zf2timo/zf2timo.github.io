---
title: Supporting Zend ServiceManager Factories for V2 and V3
tags: [zend, factory]
categories: [development]

---

When creating a module it can be useful to support Zend ServiceManager Version V2 and V3.
I dealt with this problem a few days ago: my module had to work in two application based - one based on Zend 2.4 and the
other one based on Zend 3.0. When looking at the `FactoryInterface` in both version, we easily see the problem:

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
you can't implement both Interfaces. If you do so, one of the versions will always be missing and it leads into a 
PHP Fatal Error.

To solve the problem just implement none of the `FactoryInterface`'s and manually create the methods as seen in the 
example below:
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
Please notice, I also removed the type Hinting from the methods. This allows us to pass the ServiceManger V2 into the
`__invoke` method.

## Why dose it works

This is really simple. The ServiceManager checks in both versions if the instantiated Factory supports the required method.
If the method exists in the installed version, the ServiceManager assumes that the method is the factory method and executes it.
