---
title: Meta Box Tabs
---

## Overview

The Meta Box Tabs extension is an UI extension for Meta Box, which helps you to put custom fields into tabs for a better view. It's extremely useful when you have many custom fields (which can lead to a super long page).

Screenshot:

![meta box tab styles](https://i2.wp.com/metabox.io/wp-content/uploads/2014/07/meta-box-tabs-icon-left.png)

The tabs can be used with [columns](https://metabox.io/plugins/meta-box-columns/) to structure your fields in a better way.

For more information, please see the [extension page](https://metabox.io/plugins/meta-box-tabs/).

{% include installation.html %}

## Usage

Make sure you know how to [create meta boxes](/creating-meta-boxes/) and [fields](/field-settings/) before continue!

To create tabs for your meta box, you need to add these parameters to your meta box settings:

### `tabs`

List of tabs, in one of the following formats:

- `'tab-id' => 'Tab label'`, or
- `'tab-id' => ['label' => 'Tab label', 'icon' => 'Tab icon']`

Where `tab-id` will be used in fields (below) to put fields under a tab.

If use the 2nd format, then `icon` is the tab icon. Icons are taken from [Dashicons](https://developer.wordpress.org/resource/dashicons/). The value of `icon` is the class name of Dashicons, e.g. `dashicons-email`.

If you want to use another icon (not Dashicons), either:

- set `icon` to the font icon class name. For example, if you want to use Font Awesome, set `'icon' => 'fa fa-home'`. Note that you have to enqueue the CSS for your custom font icon yourself. The plugin only supports Dashicons by default.
- set `icon` to URL of icon image, in case you want to use a custom image for tab icon

(Take a look at demo code to see how to implement)

Then for each field in the meta box, you need to specify which tab it belongs to by adding a parameter `'tab' => 'tab-id'` where `tab-id` is one of the tab IDs you have registered above.

### `tab_style`

Tab style. There are 3 styles available: `default` (like tabs for Categories), `box` (like tabs for Visual and Text modes of the main editor) or `left` (like tabs in Help screen). Optional.

![tab styles](https://i.imgur.com/7Oi5dx1.jpg)

### `tab_wrapper`

Whether or not show the meta box wrapper around tabs. Default is `true`. If set to `false`, then the wrapper will be hidden.

{% include alert.html type="warning" content="This parameter is deprecated. Please use `'style' => 'seamless'` parameter for the meta box instead." %}

![tab wrapper](https://i.imgur.com/IPzfyEY.jpg)

## Sample code

```php
add_filter( 'rwmb_meta_boxes', function ( $meta_boxes ) {
    // 1st Meta Box
    $meta_boxes[] = array(
        'title'     => 'Meta Box Tabs Demo',

        // List of tabs, in one of the following formats:
        // 1) key => label
        // 2) key => array( 'label' => Tab label, 'icon' => Tab icon )
        'tabs'      => array(
            'contact' => array(
                'label' => 'Contact',
                'icon'  => 'dashicons-email', // Dashicon
            ),
            'social'  => array(
                'label' => 'Social Media',
                'icon'  => 'dashicons-share', // Dashicon
            ),
            'note'    => array(
                'label' => 'Note',
                'icon'  => 'https://i.imgur.com/nJtag1q.png', // Custom icon, using image
            ),
        ),

        // Tab style: 'default', 'box' or 'left'. Optional
        'tab_style' => 'default',

        // Show meta box wrapper around tabs? true (default) or false. Optional
        'tab_wrapper' => true,

        'fields'    => array(
            array(
                'name' => 'Name',
                'id'   => 'name',
                'type' => 'text',

                // Which tab this field belongs to? Put tab key here
                'tab'  => 'contact',
            ),
            array(
                'name' => 'Email',
                'id'   => 'email',
                'type' => 'email',
                'tab'  => 'contact',
            ),
            array(
                'name' => 'Facebook',
                'id'   => 'facebook',
                'type' => 'text',
                'tab'  => 'social',
            ),
            array(
                'name' => 'Note',
                'id'   => 'note',
                'type' => 'textarea',
                'tab'  => 'note',
            ),
        ),
    );

    // 2nd Meta Box: Tab style - boxed
    $meta_boxes[] = array(
        'title'     => 'Meta Box Tabs 2',
        'tabs'      => array(
            'bio'      => 'Biography',
            'interest' => 'Interest',
        ),
        'tab_style' => 'box',
        'fields'    => array(
            array(
                'name' => 'Bio',
                'id'   => 'bio',
                'type' => 'textarea',
                'tab'  => 'bio',
            ),
            array(
                'name' => 'Interest',
                'id'   => 'interest',
                'type' => 'textarea',
                'tab'  => 'interest',
            ),
        ),
    );

    // 3rd Meta Box: Tab style - left
    $meta_boxes[] = array(
        'title'     => 'Meta Box Tabs 3',

        'tabs'      => array(
            'bio'      => 'Biography',
            'interest' => 'Interest',
            'job'      => 'Job',
        ),
        'tab_style' => 'left',
        'fields'    => array(
            array(
                'name' => 'Bio',
                'id'   => 'bio',
                'type' => 'textarea',
                'tab'  => 'bio',
            ),
            array(
                'name' => 'Interest',
                'id'   => 'interest',
                'type' => 'textarea',
                'tab'  => 'interest',
            ),
            array(
                'name' => 'Job Description',
                'id'   => 'job_desc',
                'type' => 'textarea',
                'tab'  => 'job',
            ),
        ),
    );

    // 4th Meta Box: No wrapper
    $meta_boxes[] = array(
        'title'       => 'Meta Box Tabs 4',
        'tabs'        => array(
            'contact' => array(
                'label' => 'Info',
                'icon'  => 'dashicons-email',
            ),
            'social'  => array(
                'label' => 'Social',
                'icon'  => 'dashicons-share',
            ),
        ),
        'tab_style'   => 'box',
        'tab_wrapper' => false,
        'fields'      => array(
            array(
                'name' => 'Name',
                'id'   => 'name2',
                'type' => 'text',
                'tab'  => 'contact',
            ),
            array(
                'name' => 'Google+',
                'id'   => 'googleplus2',
                'type' => 'text',
                'tab'  => 'social',
            ),
        ),
    );

    return $meta_boxes;
} );
```
