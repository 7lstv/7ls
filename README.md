# 7ls
Web browser interface and web content language for Roku devices.

The 7LS Browser is an internet web browser designed for Roku TV devices.
It is designed to allow fast and easy page creation tailored around the capabilities of the Roku platform.

The 7LS browser is a uniquely designed application that allows developers to push content from the internet to an end users Roku device. It uses an XML approach to codify the user application interface for both visual and logical functionality.

An internet resource is fetched from the web using a standard address format.

(http://domain.com/resource.rxml)

Once the resource is downloaded, it is read from top to bottom in a single pass creating objects as it encounters them. Because of this, care must be taken to ensure objects have been created before being referenced by other objects.

One major deviation from standard XML is how children objects are created. Normally children would be nested within the definition of a parent.

<parent
   attribute1="somevalue"
   >
   <child
      attribute1="anothervalue"
      >
   </child>
</parent>

Children objects must be defined as a separate node with an explicit reference to the parent in the "parentid" attribute. If no "parentid" is designated, it is assumed that the node is a "top" level node.

<node
   objtype="node"
   id="parent"
   >
</node>
<node
   objtype="node"
   parentid="parent"
   id="child"
   >
</node>

In addition to this requirement that child nodes be defined outside of the parent node definition, all nodes must have an "objtype" attribute.

There are two main types of objects:
   Renderable objects
   non-Renderable objects

Renderable objects can appear as visual representations on the GUI (graphical user interface). Some examples would be rectangles, images, and text. Non-Renderable objects have no visual representation. They do their work in the background. 

Some examples of these would be variables, events, methods, controls, containers and timers.

An RXML file is a plain text file that uses element tags to organize the contents of a page for the browser display. RXML files are read sequentially from top to bottom. An RXML file has only two types of tags. The first tag is the "<contents>" tag and the file is closed out with the "</contents>" tag.

sample.rxml
<contents>
...
</contents>

The second element type is defined by the "<node>" tag. It has to be ended with a "</node>" tag and this node element is placed between the content tags.

sample.rxml
<contents>
   <node
      objtype="favicon"
      uri="pkg:/images/logo2.png"
      remark="my website icon"
      >
   </node>
</contents>

The example above tells the browser to create a "favicon" object and its "uri" property is to be set to "pkg:/images/logo2.png" on creation. A node object can have many properties. Some properties are required while others are optional.

Each object is created from the node as the node elements are read from the file. This means that the order of the nodes in the file are relevant to the output of the browser. For example: if a label object is defined from a node and its location properties put it 100 pixels to the right and 100 pixels down, and then a rectangle node is defined from the 0,0 position 400 pixels wide and 400 pixels long, the rectangle would cover the label and the label would not be seen. Where as is the rectangle were defined first, the text would be displayed on top of the rectangle.

This concept applies to all elements of the RXML file. The above example demonstrates the effect this has on two renderable objects. Renderable objects are objects that can be displayed on the devices display. As well as this type of object, the browser supports other types of objects. For instance, a "variable" object type holds data that can be later used to plug in to the value of the property of a "renderable" object if the variable has been defined before the renderable object has been created.

Another group of object types are the "event observer objects". Objects of this type are what make the page functional. A "focus event observer" could be created to perform some task when its element on the page receives focus. The "key event observer" likewise can perform a function based on the button pressed of the remote control. The third type of observer object is the "change event observer". It causes an action when a property value of its parent is changed.

Some objects require a parent object, some can optionally have a parent, and some cannot have a parent, but before we get into objects and properties we need to look more at the file structure.

	
As stated above, the first and always required tag is the "<contents>" tag. The file must also always end with the closing tag "</contents>". In between the contents tags are the "<node>" tags. Each node tag defines an object that will be handled by the browser. Node elements must be ended with a "</node>" tag. The properties of the object are defined within the node element.

The first and always required attribute is the "objtype" attribute. It tells the browser the type of object it is creating. Object types fall into two categories, browser specific and Roku specific objects.

Since this xml structure differs from standard xml structure, in that you don't nest objects within other objects, the second browser attribute when needed would be the "parentid" attribute. This only applies to a Roku specific object that would be the child of a previously defined Roku object.

After the browser attributes are defined, the Roku object specific attributes may be defined.

<contents>
<node
   property1="my name is node1"
   property2="some value"
   property3="optional value"
   property4="another optional value"
   >
</node>
.
.
.
<node
   property1="my name is node47"
   property2="some required value"
   property3="another required value"
   >
</node>
</contents>

Any number of nodes may be defined within the content and each node may be one of many types of objects. Each different object may have object specific properties. The RXML language requires some objects to have browser specific properties. "objtype" is a browser specific property and directs the browser as to the type of object it will be creating. The order the properties defined in the node is not forced, nor is the case of the text value, but a good practice is to enter the browser specific properties first followed by the object specific properties and then the optional programatic properties last.

The value of the properties must be quoted. You may use single or double quotes as needed. HTML character codes may be used as well.

For example:
property="This &#38; is an Ampersand"

would be stored to the property value as:
"This & is an Ampersand".

The GUI is made up of two parts. The browser controls and the browser screen.

The browser controls contain the address bar and the control icons: bookmarks, back, forward, home, refresh, and help. The browser controls are activated by pressing the asterisk ("*") key on the input device. These controls provide for some of the standard functions found in a browser.

The browser screen is the graphical area where the content will be displayed. This area is 1152 pixels wide and 600 pixels high. Objects may be placed anywhere inside or outside of this area, but objects outside or partially outside of this area will be clipped to the extents of this area.

The standard input device for the Roku is the remote control. Other options include mobile apps for smart phones and even an app for your PC or laptops and tablets.

When a resource is loaded from the internet, the browser directs inputs to the loaded resource. All inputs excepting the asterisk, home, and channel store keys are available to the resource to control and interact between the user and the resource.

In the context of this browser, the only key predefined is the asterisk key. Pressing this key initiates the browser controls. Once the browser controls have focus, the input device is used to navigate, invoke, and interact with the browser functions.

Input monitoring is set up by defining a keyeventobserver node. This "action" node defines which object of the browser screen to monitor and for which key press and then what action to take place if that event occurs.

There are two groups of objects.

Device Objects:

Device objects include most of the objects native to the Roku device. Each device object contain some browser specific properties that direct the browser as to how to apply these objects in the device context.

Browser Objects:

Browser objects are objects that without browser interpretation would not perform any function within the device context. A browser object may create one or more "device objects" in the device context and interrelate them to other objects or functions of the browser.

"favicon" - Defines the icon that will be displayed at the left of the address bar.

"bookmark" - Tells the browser how the webpage would like to be bookmarked.

"topnode" - Allows for access to navigate from one page or site to another.

"variable" - Creates one "string", "numeric", "logical", "array", or "associative array" variable in the devices memory space.

"variables" - Create multiple variables at one time. It does not support array type variables.

"keyeventobserver" - Sets up the logic to monitor and act when a specific remote control button is pressed while an object has focus.

"changeeventobserver" - Sets up the logic to monitor and act when a value change occurs in an objects property.

"focuseventobserver" - Sets up the logic to monitor and act when an object receives focus.

"focuser" - Explicitly directs focus to an object after the page loads.

Events are the occurrence of a particular happening with regard to the browser. There are three events that the browser monitors. The most common event occurs when the user presses a key on the remote. The next event would be when an object gains focus. The third event monitored is when the field value of an object changes.

Reacting to these events is where the browser becomes functional. For example: when the user presses the "down" key on the remote, it is possible to set the focus to the next object on the page. When this object gains focus, it can set off a series of actions that change the pages appearance. It could also store a value to a variable or to the field value of another object which may itself change another element of the page. Each chain of actions caused by an event can be considered a "logic block".

A logic block must have an initiator, the event. It must also have one or more actions to process and it must have a target to perform the action on. The logic block is defined from the RXML file in the order in which the steps are encountered. When initiated, the steps are performed one by one until the logic block has run out of steps to perform.

Source properties define the source of the data that will be acted on. Only certain actions require source data. When required, at least one source property must be defined for each action. Source properties allow for multiple entries semicolon delimited.

sourceNode - required if action requires and no other source is defined

sourceField - required if sourceNode is defined

When used, the sourceNode refers to a previously defined device node object such as a label node. The value of its property defined by the sourceField is then used as the data source.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcenode="rectangle1"
   sourcefield="translation"
   .
   .
   .
   >
</node>

sourceVariable - required if no other source is defined

When used, the source data is gotten from this previously defined variable.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevariable="myvar1"
   .
   .
   .
   >
</node>

sourceElement - optional for sourceField or sourceVariable definition

If the data type of the sourceField or the sourceVariable is an array, the element of that array can be defined and will become the data source. Array elements start at number zero.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcenode="rectangle1"
   sourcefield="translation"
   sourceelement="0"
   .
   .
   .
   >
</node>

sourceKey - optional for sourceField or sourceVariable definition

If the data type of the sourceField or the sourceVariable is an associative array, the key of that array can be defined and the data associated to that key will become the data source.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevariable="myvar1"
   sourcekey="left"
   .
   .
   >
</node>

sourceValue - required if no other source is defined

The data source can be defined directly as a literal.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevalue="75"
   .
   .
   .
   >
</node>

Target properties specify what target will be acted on or for some actions define where the source data will be stored after it has been acted on. At least one target property must be defined for each action occurring. Targets may refer to the same object as the source or may refer to a different object. Target properties allow for multiple entries semicolon delimited.

targetNode - required if no other target is defined

targetField - required if targetID is defined and the action returns a value

The targetNode can be used in conjunction with the targetField to define where the data will be placed after it has been acted on.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevalue="0xc0c0c0"
   targetNode="rect1"
   targetField="color"
   .
   .
   .
   >
</node>

targetVariable - required if no other target is defined and the action returns a value

The targetVariable refers to a previously created variable. It is only used if the action specified returns a value. This will be where the processed data will be stored.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevalue="0xc0c0c0"
   targetVariable="myvar1"
   .
   .
   .
   >
</node>

targetElement - optional for targetField or targetVariable definition and the action returns a value

If the targetVariable or targetField is an array type, the targetElement can be defined so as to store the processed data at a specific element of the array.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevalue="75"
   targetNode="rect1"
   targetField="translation"
   targetElement="0"
   .
   .
   >
</node>

targetKey - optional for targetField or targetVariable definition and the action returns a value

If the targetVariable or targetField is an associative array type, the targetKey can be defined to store the processed data for that key within the array.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="OK"
   sourcevalue="75"
   targetVariable="myvar1"
   targetKey="left"
   .
   .
   .
   >
</node>

The targetaction defines what action will be performed. There is a set list of actions. One key press can trigger multiple actions to be processed. They will be processed in the order they are defined. Targetaction strings are case insensitive.

<node
   objtype="keyEvent"
   parentid="label1"
   keyevent="left"
   targetaction='setfocus'
   .
   >
</node>
