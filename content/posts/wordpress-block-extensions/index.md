---
title: "Wordpress Block Editor Plugin Extensions"
date: 2021-12-23T17:40:22Z
draft: false
---

WordPress' Block Editor is great for creating User Interfaces for configuring content that in the past would have been created via a shortcode. I have a plugin called [A-Z Listing](https://wordpress.org/plugins/a-z-listing) that I have ported to the Block Editor. This plugin doesn't pay for itself, however, so I also need to create addons or extensions that augment the Open Core plugin from WordPress.org to provide extra functionality for a modest fee.

> How do we augment a Block in the WordPress block editor with extensions?

Thankfully we can add our extensions using the inbuilt Block Editor features and its supporting libraries with "Filters" and "SlotFills".

## The Javascript

Registering the plugin via JS is fairly typical. We won't use the `blocks.json` automatic registration mechamism but instead do it programmatically so that we can use filters when registering everything.

### Registration

We need to ensure that the extensions have fully loaded, so we wait for the `domReady` event.

```js
import { __ } from '@wordpress/i18n';
import domReady from '@wordpress/dom-ready';
import { createBlock, registerBlockType } from '@wordpress/blocks';

import edit from './edit';
import globalAttributes from './attributes.json';

domReady( () => {
	const attributes = applyFilters( 'a_z_listing_attributes', globalAttributes );

	registerBlockType( 'a-z-listing/block', {
		title: __( 'A-Z Listing', 'a-z-listing' ),
		category: 'widgets',
		icon,
		supports: {
			align: true,
			html: false,
		},
        attributes,
		edit,
		save: () => null,
	} );
} );
```

Our extensions will use the filter `a_z_listing_attributes` to add extra supported parameters/attributes to the block so that everything gets saved correctly. Without this filter and augmentation by the extensions their settings will be silently ignored when saving.

### Slot Fills

We will create a Redux `store` to allow access to our slotfills:

```js
import { registerStore } from '@wordpress/data';

import DisplayOptions from '../components/DisplayOptions';
import ItemSelection from '../components/ItemSelection';
import AZInspectorControls from '../components/AZInspectorControls';

registerStore( 'a-z-listing/slotfills', {
	reducer( state = {} ) {
		return state;
	},
	actions: {},
	selectors: {
		getDisplayOptions() {
			return DisplayOptions;
		},
		getItemSelection() {
			return ItemSelection;
		},
		getInspectorControls() {
			return AZInspectorControls;
		},
	},
} );
```

Each of the three files imported here are very similar, so I'll only show one:

```js
import { createSlotFill } from '@wordpress/components';

export const { Fill, Slot } = createSlotFill( 'AZListingDisplayOptions' );

const DisplayOptions = ( { children } ) => <Fill>{ children }</Fill>;

DisplayOptions.Slot = Slot;

export default DisplayOptions;
```

In our block's edit component we include the SlotFills:

```js
import { __ } from '@wordpress/i18n';

const A_Z_Listing_Edit = ( { attributes, setAttributes } ) => {
    const inspectorControls = (
		<InspectorControls>
			<AZInspectorControls.Slot>
                { ( fills ) => (
                    <>
                        { /* ... Other controls here ... */ }
                        
                        { /* The next line includes the components from the extension plugins: */ }
                        { fills }
					</OtherComponentsHere>
				) }
			</AZInspectorControls.Slot>
		</InspectorControls>
    );

    return (
		<>
			{ inspectorControls }
            <ServerSideRender
					block="a-z-listing/block"
					attributes={ attributes }
					LoadingResponsePlaceholder={ () => <Spinner /> }
					ErrorResponsePlaceholder={ () => (
						<Placeholder
							icon={ pin }
							label={ __( 'A-Z Listing', 'a-z-listing' ) }
						>
							{ __( 'Error Loading the listing...', 'a-z-listing' ) }
						</Placeholder>
					) }
					EmptyResponsePlaceholder={ () => (
						<Placeholder
							icon={ pin }
							label={ __( 'A-Z Listing', 'a-z-listing' ) }
						>
							{ __(
								'The listing has returned an empty page. This is likely an error.',
								'a-z-listing'
							) }
						</Placeholder>
					) }
				/>
        </>
    );
} );

export default A_Z_Listing_Edit;
```

### Extension plugins

Each of our extension plugins then hooks into the filter to add their own parameters:

```js
import { addFilter } from '@wordpress/hooks';

addFilter( 'a_z_listing_attributes', 'a-z-listing/proper-nouns', ( globalAttributes ) => ( { ...globalAttributes, ...attributes } ) );
```

And uses a higher-order functional component to access the SlotFill to insert the configuration UI for the extensions' parameters:

```js
import { __ } from '@wordpress/i18n';
import { withSelect } from '@wordpress/data';
import { createHigherOrderComponent } from '@wordpress/compose';

import attributes from './attributes.json';

const ProperNounsPlugin = createHigherOrderComponent( ( HostElement ) => {
	return withSelect( (select) => {
		const { getDisplayOptions } = select( 'a-z-listing/slotfills' );
		return { getDisplayOptions };
	} )( ( props ) => {
		if ( props.name !== 'a-z-listing/block' ) {
			return (
				<HostElement { ...props }/>
			);
		}

		const { getDisplayOptions, attributes, setAttributes } = props;
		const DisplayOptions = getDisplayOptions();
		return (
			<>
				<HostElement { ...props }/>
				<AZInspectorControls>
                    { /* Anything added here will be added to the main plugin's slot named `AZInspectorControls` */ }
					<ToggleControl
                        label={ __( 'Enable proper nouns extension' ) }
                        checked={ !!attributes['proper-nouns'] }
                        onChange={ ( value ) =>
                            setAttributes( { 'proper-nouns': value } )
                        }
                    />
                    { /* ... Other controls here ... */ }
				</AZInspectorControls>
			</>
		);
	} );
} );

addFilter( 'editor.BlockEdit', 'a-z-listing/block', ProperNounsPlugin );
```

## PHP

The PHP side is similar; we augment the attributes via a filter.

### Main plugin

```php
<?php
$attributes = json_decode( file_get_contents( dirname( A_Z_LISTING_PLUGIN_FILE ) . '/scripts/blocks/attributes.json' ), true );
$attributes = apply_filters( 'a_z_listing_get_gutenberg_attributes', $attributes );

register_block_type(
    'a-z-listing/block',
    array(
        'editor_script'   => 'a-z-listing-block-editor',
        'editor_style'    => 'a-z-listing-block-editor',
        'style'           => 'a-z-listing-block',
        'render_callback' => array( $this, 'render' ),
        'attributes'      => $attributes,
    )
);
```

### Extension plugins

```php
<?php
add_filter( 'a_z_listing_get_gutenberg_attributes', 'add_proper_nouns_gutenberg_attributes' );

function add_proper_nouns_gutenberg_attributes( $attributes ) {
    $additional_attributes = json_decode( file_get_contents( trailingslashit( plugin_dir_path( __DIR__ ) ) . 'scripts/blocks/attributes.json' ), true );
    return wp_parse_args( $attributes, $additional_attributes );
}
```

## Handling the attributes

### Main plugin

We pass the attributes into another filter in PHP so that the extensions may use their parameters accordingly:

```php
<?php
function handle_a_z_shortcode_and_block( $attributes = array() ) {
    do_action( 'a_z_listing_shortcode_start', $attributes );

    ... // rest of shortcode function
}
```

### Extension plugins

```php
<?php
add_action( 'a_z_listing_shortcode_start', 'a_z_proper_nouns_handler', 10, 1 );

function a_z_proper_nouns_handler( $attributes = array() ) {
    $attributes = shortcode_atts(
        array(
            'fullname-suffixes' => '',
            'proper-nouns'      => 'off',
        ),
        $attributes,
        'a-z-listing-proper-nouns'
    );

    ... // Do something useful here
}
```
