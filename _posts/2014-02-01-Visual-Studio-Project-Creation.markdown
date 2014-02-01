---
layout: post
title:  "Visual Studio Project Creation"
date:   2014-02-01 23:43:31
categories: update
comments: true
---

Follow these steps for setting up a project in Visual Studio for developing stuffs using SFML:
(These assumes that you are using Windows platform)

`1. Download SFML`<br>

Download SFML-2.1 [Here]

`2. Install SFML`<br>

Copy SFML-2.1 folder in C Drive. 

`3. Follow these steps :`

<img src="http://gdc-ceg.github.io/images/1.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/2.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/3.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/4.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/5.PNG"><br><br>

After creating a `.cpp` file, paste these contents into it:

{% highlight ruby%}
{% raw %}
#include <SFML/Graphics.hpp>

int main()
{
    sf::RenderWindow window(sf::VideoMode(200, 200), "SFML works!");
    sf::CircleShape shape(100.f);
    shape.setFillColor(sf::Color::Green);

    while (window.isOpen())
    {
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
                window.close();
        }

        window.clear();
        window.draw(shape);
        window.display();
    }

    return 0;
}
{% endraw %}
{% endhighlight %}
<br><br>

<img src="http://gdc-ceg.github.io/images/6.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/7.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/8.PNG"><br><br>

<img src="http://gdc-ceg.github.io/images/t.PNG"><br><br>

`NOTE: ` If you are using Visual Studio 2013 or higher versions, change `Enable Incremental Linking` to `Yes`.<br> 

<img src="http://gdc-ceg.github.io/images/10.PNG"><br><br>

Once you finish these and before debugging the project, copy the contents of `bin` folder in `SFML-2.1` directory (that you already have in C Drive) to the `debug` folder in your `Project's Directory`. This is very important step!


After that `debug` the project and Here it works! :)

<img src="http://gdc-ceg.github.io/images/11.PNG"><br>

For More reference visit [This]

If you have any problems in setting up the project, do comment here or in our Facebook group!

[Here]: http://www.sfml-dev.org/download/sfml/2.1/
[This]: http://www.sfml-dev.org/tutorials/2.1/start-vc.php
