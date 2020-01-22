---
title: Creating meta boxes
---

## Hook

To create a meta box, you need to hook to the filter `rwmb_meta_boxes` to add meta box settings. This filter accepts 1 parameter - the array of meta box settings.

The code below registers a simple meta box:

```php
add_filter( 'rwmb_meta_boxes', 'prefix_register_meta_boxes' );
function prefix_register_meta_boxes( $meta_boxes ) {
    $meta_boxes[] = array(
        'title'      => 'Personal Information',
        'post_types' => 'post',

        'fields' => array(
            array(
                'name'  => 'Full name',
                'desc'  => 'Format: {First Name} {Last Name}',
                'id'    => 'prefix_name',
                'type'  => 'text',
            ),
        )
    );

    // Add more meta boxes if you want
    // $meta_boxes[] = ...

    return $meta_boxes;
}
```

## Meta box settings

Each meta box has the following settings:

Name|Description
--|--
`id`|Meta box ID, must be unique. Optional. If it's absent, it will be generated from `title` using `sanitize_title` function.
`title`|Meta box title. Required.
`post_types`|Custom post types which the meta box is for. There can be an array of multiple custom post types or a string for the single post type. Must be in lowercase (like the slug). Optional. Default: `post`.
`context`|Where the meta box is displayed. See below for list of meta box contexts. Optional.
`style`|Whether to keep the default WordPress meta box style (`default`) or remove the wrapper box and display the fields seamlessly (`seamless`).
`closed`|Whether to collapse the meta box when page loads? Optional. Default: `false`.
`priority`|Priority within the context where the box is displayed (`high` or `low`). Optional. Default: `high`.
`default_hidden`|Hide the meta box by default (`true` or `false`)? The meta box can be toggled using the checkbox option in screen Help (on the top right). Optional. Default `false`.
`autosave`|Auto save the custom fields' values (like post content and title)? Optional. Default: `false`.
`media_modal`|Add custom fields to media modal when viewing/editing an attachment. Works only when `post_types` is or contains `attachment`. Optional. Default `false`.
`class`|Custom CSS class for the meta box wrapper. Optional.
`fields`|Array of fields. See section below.

{% include alert.html type="info" content="Because of some limitations in the media modal, only simple fields such as text, select, radio, checkbox work. Other fields that require custom JavaScript don't work, unfortunately." %}

### Contexts

The plugin supports the following contexts (locations) where a meta box can appear:

Name|Description
`normal`|Below the post editor. This is the default value.
`advanced`|Below the `normal` section.
`side`|On the right sidebar.
`form_top`|Top of the post form, before post title. Added in version 4.13.0.
`after_title`|After post title. Added in version 4.13.0.
`after_editor`|After the post content editor, but before `normal` section. Added in version 4.13.0.
`before_permalink`|Before permalink. Added in version 4.13.0.

This is the screenshot how meta boxes appear in `form_top` and `after_title` locations (used with `seamless` style):

![meta box locations](https://i.imgur.com/kBKbS3wl.png)

{% include alert.html type="warning" content="Gutenberg editor does NOT support extra contexts. Only normal, advanced and side contexts are supported. If you use Gutenberg, use these contexts only." %}

### Contexts not working

There are some situations that the context doesn't work as expected. That is probably because you have dragged and dropped the meta boxes to reorder them? If you have, then WordPress will save the position/location of them and makes the `context` parameter not working. The order of meta boxes is saved in the user meta `meta-box-order_{screen id}` as follows:

![meta box order](https://i.imgur.com/A7bkxT9.png)

In this case, deleting this user meta from the database will make the context work again.

## Fields

Fields are added to a meta box via the key `fields`. Each field is an array of settings. In the example above, the text field is defined via:

```php
array(
    'name'  => 'Full name',
    'desc'  => 'Format: {First Name} {Last Name}',
    'id'    => $prefix . 'name',
    'type'  => 'text',
),
```

Meta Box supports more than 40 field types. All fields share some common settings like `id`, `type`, but also offer unique settings per field type. See [this guide](/field-settings/) for more details.

{% include alert.html content="Field ID is used as the meta key and the field value is used as the meta value when saving into the post meta table." %}

You can (should) add prefix to field IDs to prevent from using the same ID with other scripts. If you want to hide the fields in the default WordPress **Custom Fields** meta box, use underscore (`_`) as the prefix.
