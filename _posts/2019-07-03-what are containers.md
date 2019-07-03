### what are containers?

A software container is a pretty abstract thing and thus it might help if we start with an analogy that should be pretty familiar to most of the readers. The analogy is a shipping container in the transportation industry. Throughout history, people have been transporting goods from one location to another by various means. Before the invention of the wheel, goods would most probably have been transported in bags, baskets, or chests on the shoulders of the humans themselves, or they might have used animals such as donkeys, camels, or elephants to transport them.

With the invention of the wheel, transportation became a bit more efficient as humans would built roads on which they could move their carts along. Many more goods could be transported at a time. When we then introduced the first steam-driven machines, and later gasoline driven engines, transportation became even more powerful. We now transport huge amounts of goods in trains, ships, and trucks. At the same time, the type of goods became more and more diverse, and sometimes complex to handle.

In all these thousands of years, one thing did not change though, and that was the necessity to unload the goods at the target location and maybe load them onto another means of transportation. Take, for example, a farmer bringing a cart full of apples to a central train station where the apples are then loaded onto a train, together with all the apples from many other farmers. Or think of a winemaker bringing his barrels of wine with a truck to the port where they are unloaded, and then transferred to a ship that will transport the barrels overseas.

This unloading from one means of transportation and loading onto another means of transportation was a really complex and tedious process. Every type of good was packaged in its own way and thus had to be handled in its own way.  Also, loose goods risked being stolen by unethical workers, or goods could be damaged in the process.

Then, there came the container, and it totally revolutionized the transportation industry. The container is just a metallic box with standardized dimensions. The length, width, and height of each container is the same. This is a very important point. Without the world agreeing on a standard size, the whole container thing would not have been as successful as it is now.

Now, with standardized containers, companies who want to have their goods transported from A to B package those goods into these containers. Then, they call a shipper which comes with a standardized means for transportation. This can be a truck that can load a container or a train whose wagons can each transport one or several containers. Finally, we have ships that are specialized in transporting huge amounts of containers. The shippers never need to unpack and repackage goods. For a shipper, a container is a black box and they are not interested in what is in it nor should they care in most cases. It is just a big iron box with standard dimensions. The packaging of goods into containers is now fully delegated to the parties that want to have their goods shipped, and they should know best on how to handle and package those goods.

Since all containers have the same standardized shape and dimensions, the shippers can use standardized tools to handle containers, that is, cranes that unload containers, say from a train or a truck, and load them onto a ship or vice versa. One type of crane is enough to handle all the containers that come along over time. Also, the means of transportation can be standardized, such as container ships, trucks, and trains.

Because of all this standardization, all the processes in and around shipping goods could also be standardized and thus made much more efficient than they were before the age of containers.

I think by now you should have a good understanding of why shipping containers are so important and why they revolutionized the whole transportation industry. I chose this analogy purposefully, since the software containers that we are going to introduce here fulfill the exact same role in the so-called software supply chain as shipping containers do in the supply chain of physical goods.

In the old days, developers would develop a new application. Once that application was completed in the eyes of the developers, they would hand this application over to the operations engineers that were then supposed to install it on the production servers and get it running. If the operations engineers were lucky, they even got a somewhat accurate document with installation instructions from the developers. So far so good, and life was easy.

But things got a bit out of hand when in an enterprise, there were many teams of developers that created quite different types of applications, yet all needed to be installed on the same production servers and kept running there. Usually, each application has some external dependencies such as which framework it was built on or what libraries it uses and so on. Sometimes, two applications would use the same framework but in different versions that might or might not be compatible between each other. Our operations engineer's life became much harder over time. They had to be really creative on how they could load their ship, which is of course their servers with different applications without breaking something.

Installing a new version of a certain application was now a complex project on its own and often needed months of planning and testing. In other words, there was a lot of friction in the software supply chain. But these days, companies rely more and more on software and the release cycles become shorter and shorter. We cannot afford anymore to just have a new release maybe twice a year. Applications need to be updated in a matter of weeks or days, or sometimes even multiple times per day. Companies that do not comply risk going out of business due to the lack of agility. So, *what's the solution?*

A first approach was to use **virtual machines** (**VMs**). Instead of running multiple applications all on the same server, companies would package and run a single application per VM. With it, the compatibility problems were gone and life seemed good again. Unfortunately, the happiness didn't last for long. VMs are pretty heavy beasts on their own since they all contain a full-blown OS such as Linux or Windows Server and all that for just a single application. This is as if in the transportation industry you would use a gigantic ship just to transport a truck load of bananas. What a waste. That can never be profitable.

The ultimate solution to the problem was to provide something much more lightweight than VMs but also able to perfectly encapsulate the goods it needed to transport. Here, the goods are the actual application written by our developers plus (and this is important) all the external dependencies of the application, such as framework, libraries, configurations, and more. This holy grail of a software packaging mechanism was the Docker container.

`Developers use Docker containers to package their applications, frameworks, and libraries into them, and then they ship those containers to the testers or to the operations engineers.` For the testers and operations engineers, the container is just a black box. It is a standardized black box, though. All containers, no matter what application runs inside them, can be treated equally. The engineers know that if any container runs on their servers, then any other containers should run too. And this is actually true, apart from some edge cases which always exist.

`Thus, Docker containers are a means to package applications and their dependencies in a standardized way`. Docker then coined the phrase—*Build, ship and run anywhere*.