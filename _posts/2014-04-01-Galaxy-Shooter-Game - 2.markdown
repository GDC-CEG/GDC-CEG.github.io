---
layout: post
title:  "Basic Galaxy Shooter - 2"
date:   2014-04-01 20:03:00
categories: update
comments: true
share: true
---
Kindly look at [Basic-Galaxy-Shooter-1] before proceeding. Code is almost similar. Modify the project code wherever needed to look like the one below

`Bullets.h`<br>
{% highlight ruby%}
{% raw %}
#include "SFML\Graphics.hpp"
#include "CollisionManager.h"

bool FLAG=false; // flag to know whether collision happened or not, so that source.cpp is informed
const int VELO=20; // velocity of bullet is 20

class Bullets
{
public:
  sf::IntRect box;
  sf::RectangleShape rect; // rectangle shape to display bullet on screen
  int yVel;

  Bullets(int x,int y);
  
  void update();
  void display(sf::RenderWindow *window);

};

Bullets::Bullets(int x, int y)
{
  //initial position of bullet depends on the position of the hero ship
  box.left=x;
  box.top=y;
  box.height=10;
  box.width=2;
  
  rect.setSize(sf::Vector2f(box.width,box.height));
  rect.setFillColor(sf::Color::Blue); // color of bullet
  
  yVel=-VELO; // bullet will move up the screen . so y velocity is negative .
}

void Bullets::update()
{

  box.top+=yVel; // bullet moves up the screen
  if ( CM.CheckCollide(box) == true )
  {
    FLAG=true;
  }
}

void Bullets::display(sf::RenderWindow *window)
{
  //displaying the bullet
  rect.setPosition(box.left,box.top);
  window->draw(rect);
}

{% endraw %}
{% endhighlight %}<br><br>


`HeroShip.h`<br>
{% highlight ruby%}
{% raw %}
#include"SFML\Graphics.hpp"
#include"Bullets.h"

//Look at basic galaxy shooter -1 to understand heroship and source.cpp easily
const int VEL=10;
class HeroShip
{
public:
    sf::IntRect box;
    sf::RectangleShape rect;
    int xVel,yVel;
    
    Bullets *myBullets[10]; // 10 bullets can only be fired. can be increased if needed
    int num_of_bullets;

    HeroShip();
    void handleEvents();
    void update();
    void display(sf::RenderWindow *window);
    void shoot();

};

HeroShip::HeroShip()
{
    box.left=200;
    box.top=500;
    box.height=20;
    box.width=20;
    
    rect.setSize(sf::Vector2f(box.width,box.height));
    rect.setFillColor(sf::Color::Red);
    
    xVel=0;
    yVel=0;

    num_of_bullets=0;
}
void HeroShip::handleEvents()
{
    //top or down
    if(sf::Keyboard::isKeyPressed(sf::Keyboard::Up))
        yVel=-VEL;
    else if(sf::Keyboard::isKeyPressed(sf::Keyboard::Down))
        yVel=VEL;
    else
        yVel=0;
    //left or right
    if(sf::Keyboard::isKeyPressed(sf::Keyboard::Left))
        xVel=-VEL;
    else if(sf::Keyboard::isKeyPressed(sf::Keyboard::Right))
        xVel=VEL;
    else
        xVel=0;

    if(sf::Keyboard::isKeyPressed(sf::Keyboard::Z))
        shoot();
    
}

void HeroShip::update()
{
    box.left+=xVel;
    box.top+=yVel;

    for(int i=0;i<num_of_bullets;i++)
        myBullets[i]->update(); // updating every bullet position
}

void HeroShip::display(sf::RenderWindow *window)
{
    for(int i=0;i<num_of_bullets;i++)
        myBullets[i]->display(window); // calling display function of all bullets

    rect.setPosition(box.left,box.top);
    window->draw(rect);
}

void HeroShip::shoot()
{
    //to shoot bullets
    if(num_of_bullets<10)
    {
        myBullets[num_of_bullets]=new Bullets(box.left+box.width/2,box.top-10); // position of bullet : x = middle of hero ship ; y = 10 pixels above the top of enemy ship
        num_of_bullets++;
    }
}

{% endraw %}
{% endhighlight %}<br>

`EnemyShips.h`<br>
{% highlight ruby%}
{% raw %}
#pragma once
#ifndef ENEMYSHIPS_H
#define ENEMYSHIPS_H

#include"SFML\Graphics.hpp"

class EnemyShip
{
    public:
        sf::IntRect box;
        sf::RectangleShape rect;
        int xVel,yVel;

        EnemyShip();
        void update();
        void display(sf::RenderWindow *window);
        void changeColor();
};

EnemyShip::EnemyShip()
{
    //position of enemy ship on screen
    box.left=300;
    box.top=200;
    box.height=20;
    box.width=20;
    
    rect.setSize(sf::Vector2f(box.width,box.height));
    rect.setFillColor(sf::Color::Yellow);  // enemy ship is colored yellow
    
    xVel=0;
    yVel=0;

}

void EnemyShip::update()
{
    // since enemy ship doesnt move , we do nothing here in update
}

void EnemyShip::changeColor()
{
    rect.setFillColor(sf::Color::Black);  // when enemy ship is hit by a bullet we change its color
}
void EnemyShip::display(sf::RenderWindow *window)
{
    //enemy ship is drawn here
    rect.setPosition(box.left,box.top);
    window->draw(rect);
}

#endif

{% endraw %}
{% endhighlight %}<br>


`CollisionManager.h`<br>
{% highlight ruby%}
{% raw %}
#include "EnemyShips.h"
//to detect collisions between bullets and enemy ship

class CollisionManager
{

    sf::IntRect *esbox[10]; // enemy ship's position values. can store upto 10 ships' values. but here we use only 1
    sf::IntRect *bulletBox[10]; // to store position of bullets
public:
    int no_of_eships;
    void AssignES(EnemyShip *es); // to fill esbox[] array with enemy ship's positon values
    bool CheckCollide(sf::IntRect box); // to check collision
    
};

CollisionManager CM; // an external object for Collision manager class to use whenever needed from anywhere

void CollisionManager::AssignES(EnemyShip *es)
{
    //Assigning position of enemy ship's to the array esbox
    no_of_eships=0;
    esbox[no_of_eships]=new sf::IntRect;
    esbox[no_of_eships]->left=es->box.left;
    esbox[no_of_eships]->top=es->box.top;
    esbox[no_of_eships]->height=es->box.height;
    esbox[no_of_eships]->width=es->box.width;
    no_of_eships++;
}

bool CollisionManager::CheckCollide(sf::IntRect box)
{
    //checking boundaries of bullet and enemy ship to find whether they collide
    if(box.left > esbox[0]->left 
            && box.left < esbox[0]->left + esbox[0]->width
            && box.top > esbox[0]->top 
            && box.top < esbox[0]->top + esbox[0]->height )
     return true;
    else 
        return false;
}
{% endraw %}
{% endhighlight %}<br>


`Source.cpp`<br>
{% highlight ruby%}
{% raw %}
//Look at basic galaxy shooter - 1 to understand HeroShip.h and Source.cpp easily


#include <SFML/Graphics.hpp>
#include "HeroShip.h"
#include "EnemyShips.h"



int main()
{
    // Create the main window
    sf::RenderWindow *window;
    window=new sf::RenderWindow(sf::VideoMode(800, 600), "SFML window");
    window->setFramerateLimit(20);

    HeroShip *myship=new HeroShip();
    EnemyShip *es1=new EnemyShip(); // creating an enemy ship
    CM.AssignES(es1); // informing the position of enemy ship to collision manager
    while (window->isOpen())
    {
        // Process events
        sf::Event event;
        while (window->pollEvent(event))
        {
            // Close window : exit
            if (event.type == sf::Event::Closed)
                window->close();
        }
        //user
        myship->handleEvents();
        myship->update();
        //AI
        es1->update();
        //check flag to know whether collision happened. 
        if ( FLAG==true ) //FLAG is from Bullets.h
        {
            es1->changeColor();
            FLAG=false;
        }
        //Display
        window->clear();
        myship->display(window);
        es1->display(window);
        window->display();
    }
    return 0;
}
{% endraw %}
{% endhighlight %}<br>


You can also find this on our Github: [Galaxy-Shooter-2]


[Galaxy-Shooter-2]: https://github.com/GDC-CEG/Galaxy-Shooter-2
[Basic-Galaxy-Shooter-1]: http://gdc-ceg.github.io/update/2014/03/09/Galaxy-Shooter-Game.html
