# CodingFirst-XML
Unity package to initialises a list of your C# objects though definitions written in XML. Use it to define any game object or setting: UI-elements, playing-cards, buildings, NPCs, storage-classes, etc.

The usage can be very light, just for some configuration settings you want to have in a file in a neat structure, or you can go all-in; build you entire game with coding-only and only a single object in the scene. 

There are two main components to this:
- The Catalogue in the Scene: When the game is started all XML files will be read and objects will be placed in the catalogue for access.
- Class Files: Each XML based object has a corresponding class. An object will be generated and member variables will be fill with data from the XML. 
- The XML Files: Stores all the actual data.

Simple Example: Playing Cards
=============================

Let's start easy and imagine you want to build a playing card game. You would have a class that contains the logic about each card. So you write a class for it:

- Note that the class derives from `AnyDef`
- Each attribute that should be loaded from xml is marked with `[AnyDefLoadXMLNode]`
- Many different datatypes are supported, even custom enums like the `CardColor` enum

```c#
  class PlayingCard : AnyDef
  {
    /* PROPERTIES FROM XML */
    [AnyDefLoadXMLNode]
    public string Category = "";

    [AnyDefLoadXMLNode]
    public CardColor Color = CardColor.None;
    /* PROPERTIES FROM XML */
  }
```

Next you want to initialize a few cards, so you write them up in XML: 

- Write one block per card you want to have in the catalogue.
- Advanced tip: If you want many identical ones you would make a basic one in XML and clone the object from the catalogue in your application code

```XML
<?xml version="1.0" encoding="utf-8" ?>
<Defs>
  <ARedCard Class="PlayingCard">
    <defName>Red_Card</defName>
    <category>Red</category>
  </ARedCard>
  
  <ABlueCard Class="PlayingCard">
    <defName>Blue_Card</defName>
    <category>Blue</category>
  </ABlueCard>
</Defs>
```

How you use them is up to you, but if you just want to list them, here is an example of how you would do that:

```c#
foreach (PlayingCard pCard in XMLDefCatalogue.Instance.ListAll<PlayingCard>())
{
  Debug.Log($"Card: {pCard.DefName} -> Category: {pCard.Category}");
}
```

- XMLDefCatalogue: The catalogue mentioned earlier.
