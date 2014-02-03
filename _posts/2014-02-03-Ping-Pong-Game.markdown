---
layout: post
title:  "Ping Pong Game"
date:   2014-02-03 21:43:31
categories: update
comments: true
share: true
---

This is the code for Ping Pong game that we did in our second meet.

You can also find this on our Github: [Ping-Pong]


`Main.cpp` <br>
{% highlight ruby%}
{% raw %}
#include <iostream>

#include "SFML\Graphics.hpp"
#include "PingPong.h"

int main()
{
    // create the window to show your game 
    sf::RenderWindow *window;

  	window=new sf::RenderWindow(sf::VideoMode(600, 600),"My window");
	  window->setFramerateLimit(20);
    
    //Initialise the game objects
	  init_players();

    // run the program as long as the window is open
    while (window->isOpen())
    {
        // check all the window's events that were triggered since the last iteration of the loop
        sf::Event event;

        while (window->pollEvent(event))
        {
            // "close requested" event: we close the window
            if (event.type == sf::Event::Closed)
                window->close();
        }


        handle_input();

        update();

        perform_ai();

        // clear the window with black color
        window->clear(sf::Color::White);
	    
        // draw everything here...
        display(window);

        // end the current frame
        window->display();
    }

    return 0;
}
{% endraw %}
{% endhighlight %}<br><br>


`Pingpong.h`<br>
{% highlight ruby%}
{% raw %}
#include "SFML\Graphics.hpp"

#include <iostream>

//Rectangle for Player Slider
sf::IntRect slider_right;

//Rectangle for AI Slider
sf::IntRect slider_left;

//Rectangle for Ball
sf::IntRect ball;

//Velocity Values for Ball and Slider
const int BALL_VEL   = 10;
const int SLIDER_VEL = 10;

//Velocity variables for Player and Ball
int playerYvel = 0;

int ballYvel = 0;
int ballXvel = 0;

//Rectangle Shape for Player Slider, AI Slider and Ball respectively
sf::RectangleShape R1(sf::Vector2f(20,200));
sf::RectangleShape R2(sf::Vector2f(20,200));
sf::RectangleShape R3(sf::Vector2f(20,20));

//Initialise scores
int SCORE_PLAYER = 0,SCORE_AI = 0;

void init_players()
{
    //Assign values to Ball Rectangle
    ball.left   = 30;
    ball.top    = 30;
    ball.width  = 20;
    ball.height = 20;

    //Assign values to Player Slider Rectangle
    slider_right.left   = 550;
    slider_right.top    = 200;
    slider_right.width  = 20;
    slider_right.height = 200;

    //Assign values to AI Slider Rectangle
    slider_left.left   = 30;
    slider_left.top    = 200;
    slider_left.width  = 20;
    slider_left.height = 200;

    //Fill Colour to the Rectangle Shapes for Player Slider, AI Slider and Ball respectively
    R1.setFillColor(sf::Color::Red);
    R2.setFillColor(sf::Color::Blue);
    R3.setFillColor(sf::Color::Black);

    //Apply velocity to the Ball
    ballXvel = BALL_VEL;
    ballYvel = BALL_VEL;
}

void handle_input()
{
    //If Up arrow is pressed
    if(sf::Keyboard::isKeyPressed(sf::Keyboard::Up))
      playerYvel = -(SLIDER_VEL);

    //If Down arrow is pressed
    else if(sf::Keyboard::isKeyPressed(sf::Keyboard::Down))
      playerYvel = SLIDER_VEL;

    //If no key is pressed
    else
      playerYvel = 0;
}

void update()
{
    //If ball collides with any of the two sliders
    if(ball.intersects(slider_right) || ball.intersects(slider_left) )
      ballXvel = -(ballXvel);

    //If ball collides with the top and bottom walls
    if(ball.top < 0 || (ball.top+ball.height) > 600)
      ballYvel = -(ballYvel);

    //If ball collides with the left side wall
    if(ball.left < 0)
    {
      SCORE_PLAYER +=100;
      ballXvel      = -(ballXvel);
    }

    //If ball collides with the right side wall
    else if((ball.left+ball.width) > 600)
    {
      SCORE_AI += 100;
      ballXvel  = -(ballXvel);
    }
    
    //Change ball's position with respect to velocity
    ball.left += ballXvel;
    ball.top  += ballYvel;

    //Change Player Slider's position with respect to velocity
    slider_right.top += playerYvel;
}

void perform_ai()
{
    //If Ball is above the AI Slider 
    if(ball.top < slider_left.top)
      slider_left.top -= SLIDER_VEL;
  
    //If Ball is below the AI Slider
    else if(ball.top > (slider_left.top + slider_left.height))
      slider_left.top += SLIDER_VEL;
}

void display(sf::RenderWindow *window)
{
    //Displace the Player Slider and draw it on screen
    R1.setPosition(slider_right.left,slider_right.top);
    window->draw(R1);

    //Displace the AI Slider and draw it on screen
    R2.setPosition(slider_left.left,slider_left.top);
    window->draw(R2);
 
    //Displace the Ball and draw it on screen
    R3.setPosition(ball.left,ball.top);
    window->draw(R3);
}
{% endraw %}
{% endhighlight %}

[Ping-Pong]: https://github.com/GDC-CEG/Ping-Pong
