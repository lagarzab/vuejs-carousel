# Carousel
A simple component used to rotate through a number of slides, using a background of your choice with overlaying text.

## Usage

### `slides` Array
`slides` shoud be an array of objects which require the following fields:

* text: The text you would like displayed on the slide.
* style: The styles you would like applied to the slide.

This array should be passed to the carousel as a prop.
`slides`

You must also generate markup for the slides themselves.  You can manage the selected slide by accessing the `selectedIndex` slot prop that is returned by the component.  

Here is an example on how to use the component:
```
    <carousel :slides='slides'>
        <template #default='{ selectedIndex }' >
            <div>
                {{ slides[ selectedIndex ].text }}
            </div>
        </template>
    </carousel>
```
