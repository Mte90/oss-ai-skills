---
name: wordpress
description: Comprehensive guide for contributing to WordPress core development, including environment setup, coding standards, hooks system, database API, REST API, testing, and Trac workflow.
metadata:
  author: mte90
  version: 1.0.0
  tags:
    - wordpress
    - php
    - cms
    - open-source
    - core-contribution
    - hooks
    - testing
---

# WordPress Core Contribution

Complete guide for contributing to WordPress core development.

## Overview

WordPress is an open-source CMS written in PHP. This skill covers contributing to WordPress core.

**Key Resources:**
- Developer Docs: https://developer.wordpress.org
- Core Handbook: https://make.wordpress.org/core/handbook/
- Trac: https://core.trac.wordpress.org
- Slack: https://make.wordpress.org/chat/

## Development Environment

### Requirements

```bash
# PHP 7.4+ (8.0+ recommended)
php -v

# Node.js (for building JS)
node -v
npm -v

# Git
git --version

# Database (MySQL 5.7+ or MariaDB 10.2+)
mysql --version

# PHPUnit 8.x (for testing)
phpunit --version

# Composer (for dependencies)
composer --version
```

### Setting Up WordPress

```bash
# Clone the repository
git clone https://github.com/WordPress/wordpress-develop.git
cd wordpress-develop

# Copy config file
cp wp-tests-config-sample.php wp-tests-config.php

# Edit wp-tests-config.php
# Set database credentials and test URLs
```

### wp-tests-config.php

```php
<?php
// wp-tests-config.php

// Database credentials
define( 'DB_NAME', 'wordpress_tests' );
define( 'DB_USER', 'root' );
define( 'DB_PASSWORD', '' );
define( 'DB_HOST', 'localhost' );

// Table prefix
$table_prefix = 'wp_';

// Test URLs
define( 'WP_TESTS_DOMAIN', 'localhost' );
define( 'WP_TESTS_EMAIL', 'admin@example.org' );
define( 'WP_TESTS_TITLE', 'Test Blog' );

// Absolute path to WordPress
define( 'ABSPATH', dirname( dirname( __DIR__ ) ) . '/src/' );

// Test suite URL
define( 'WP_PHPUNIT_POLYFILLS_PATH', dirname( __DIR__ ) . '/src/wp-includes/phpunit6/polyfills.php' );
```

## Coding Standards

### WordPress Coding Standards (WPCS)

```bash
# Install WPCS via Composer
composer require --dev phpcompatibility/php-compatibility
composer require --dev wp-coding-standards/wpcs

# Create phpcs.xml
cat > phpcs.xml << 'EOF'
<?xml version="1.0"?>
<ruleset name="WordPress Coding Standards">
  <rule ref="WordPress"/>
  <rule ref="WordPress.WP.I18n"/>
  <rule ref="WordPress.Files.FileName"/>
  <rule ref="WordPress.NamingConventions"/>
  <config name="testVersion" value="5.0-"/>
</ruleset>
EOF

# Run standards check
vendor/bin/phpcs --standard=WordPress src/wp-includes/my-file.php
```

### Naming Conventions

```php
// ❌ BAD: camelCase for functions
function getUserName() {
    return get_the_title();
}

// ✅ GOOD: lowercase with underscores
function get_user_name() {
    return get_the_title();
}

// ❌ BAD: Camel_Case for classes
class My_Custom_Class {}

// ✅ GOOD: PascalCase, use prefixes
class My_Custom_Class {}

// ❌ BAD: Variables in camelCase
$myVariable = 'test';

// ✅ GOOD: Variables in lowercase with underscores
$my_variable = 'test';

// ❌ BAD: File names in CamelCase
// MyCustomClass.php

// ✅ GOOD: File names in lowercase with hyphens
// class-my-custom-class.php
```

### Spacing and Formatting

```php
// ❌ BAD: Missing spaces
if($condition){
    $value='test';
}

// ✅ GOOD: Proper spacing
if ( $condition ) {
    $value = 'test';
}

// ❌ BAD: Tabs
if ( $condition ) {
	$value = 'test';
}

// ✅ GOOD: Tabs for indentation
if ( $condition ) {
	$value = 'test';
}

// ❌ BAD: Array formatting
$array=array('one','two');

// ✅ GOOD: Multi-line arrays
$array = array(
	'one',
	'two',
);
```

### Documentation Standards

```php
/**
 * Summary of function (one line, period at end).
 *
 * More detailed description if needed.
 *
 * @since 5.0.0
 * @see  Related function name
 *
 * @param type $param_name Description.
 * @return type Description of return value.
 */
function my_function( $param_name ) {
	return 'value';
}

/**
 * Summary.
 *
 * Description spanning
 * multiple lines.
 *
 * @since 4.9.0
 * @global type $var_name Description.
 *
 * @param type $param_name Description.
 * @param type $param_two  Description.
 * @return type Description.
 */
function another_function( $param_name, $param_two = 'default' ) {
	global $var_name;

	return 'value';
}
```

## Hooks System

### Actions (Events)

```php
// Register an action hook
add_action( 'init', 'my_init_function', 10, 2 );

/**
 * Initialize plugin functionality.
 *
 * @since 1.0.0
 */
function my_init_function() {
	// Initialization code
	// Register post types, taxonomies, etc.
}

// With parameters
add_action( 'save_post', 'my_save_post_callback', 10, 3 );

/**
 * Save post metadata.
 *
 * @since 1.0.0
 *
 * @param int     $post_id Post ID.
 * @param WP_Post $post    Post object.
 * @param bool    $update  Whether this is an update.
 */
function my_save_post_callback( $post_id, $post, $update ) {
	// Check auto-save
	if ( defined( 'DOING_AUTOSAVE' ) && DOING_AUTOSAVE ) {
		return;
	}

	// Save custom data
	update_post_meta( $post_id, '_custom_meta', 'value' );
}
```

### Filters (Data Modification)

```php
// Register a filter hook
add_filter( 'the_content', 'my_content_filter', 10, 1 );

/**
 * Filter post content.
 *
 * @since 1.0.0
 *
 * @param string $content Post content.
 * @return string Modified content.
 */
function my_content_filter( $content ) {
	// Modify content
	return $content . '<p>Added content</p>';
}

// With multiple parameters
add_filter( 'wp_insert_post_data', 'my_insert_post_data_filter', 10, 2 );

/**
 * Filter post data before insertion.
 *
 * @since 1.0.0
 *
 * @param array $data    Parsed post data.
 * @param array $postarr Raw post data.
 * @return array Modified data.
 */
function my_insert_post_data_filter( $data, $postarr ) {
	// Validate and modify data
	return $data;
}
```

### Hook Priority

```php
// Priority determines execution order
// Lower priority = earlier execution

add_action( 'init', 'early_hook', 1 );      // First
add_action( 'init', 'default_hook', 10 );    // Default
add_action( 'init', 'late_hook', 20 );      // Last

// Example: Customize login form
add_action( 'login_form', 'custom_login_html', 5 ); // Early
add_action( 'login_form', 'custom_login_input', 10 ); // Default
add_action( 'login_form', 'custom_login_js', 20 ); // Late
```

### Removing Hooks

```php
// Remove default WordPress hook
remove_action( 'wp_head', 'wp_generator' );

// Remove with priority
remove_action( 'wp_enqueue_scripts', 'wp_enqueue_block_library_footer', 10 );

// Remove from class
add_action( 'wp_footer', 'remove_footer_scripts', 999 );
function remove_footer_scripts() {
	remove_action( 'wp_footer', 'wp_enqueue_scripts', 20 );
}

// Conditional removal
function my_plugin_init() {
	if ( ! current_user_can( 'manage_options' ) ) {
		remove_filter( 'the_content', 'wptexturize' );
	}
}
add_action( 'init', 'my_plugin_init' );
```

### Class Methods as Hooks

```php
class My_Plugin {
	public function __construct() {
		add_action( 'init', array( $this, 'init' ) );
		add_filter( 'the_content', array( $this, 'filter_content' ) );
	}

	/**
	 * Initialize plugin.
	 */
	public function init() {
		// Initialization code
	}

	/**
	 * Filter content.
	 *
	 * @since 1.0.0
	 *
	 * @param string $content Content.
	 * @return string Modified content.
	 */
	public function filter_content( $content ) {
		return $content;
	}
}

new My_Plugin();
```

### Dynamic Hooks

```php
// Dynamically create hooks
function create_dynamic_hooks() {
	$taxonomies = get_taxonomies( array( 'public' => true ) );

	foreach ( $taxonomies as $taxonomy ) {
		$taxonomy_name = $taxonomy->name;

		// Create dynamic filter
		add_filter(
			"{$taxonomy_name}_row_actions",
			array( $this, 'taxonomy_row_actions' ),
			10,
			2
		);
	}
}

/**
 * Add actions to taxonomy row.
 *
 * @since 1.0.0
 *
 * @param array   $actions Actions.
 * @param WP_Term $term    Term object.
 * @return array Modified actions.
 */
public function taxonomy_row_actions( $actions, $term ) {
	$actions['my_action'] = sprintf(
		'<a href="#">%s</a>',
		esc_html__( 'My Action', 'text-domain' )
	);

	return $actions;
}
```

## Query System

### WP_Query

```php
$args = array(
	'post_type'      => 'post',
	'posts_per_page' => 10,
	'paged'          => get_query_var( 'paged' ) ? get_query_var( 'paged' ) : 1,
	'orderby'        => 'date',
	'order'          => 'DESC',
	'meta_query'     => array(
		array(
			'key'     => 'featured',
			'value'   => 'yes',
			'compare' => '=',
		),
	),
	'tax_query'      => array(
		array(
			'taxonomy' => 'category',
			'field'    => 'slug',
			'terms'    => array( 'news', 'updates' ),
			'operator'  => 'IN',
		),
	),
);

$query = new WP_Query( $args );

if ( $query->have_posts() ) {
	while ( $query->have_posts() ) {
		$query->the_post();

		the_title();
		the_content();
	}

	// Pagination
	next_posts_link();
	previous_posts_link();
}

wp_reset_postdata();
```

### pre_get_posts

```php
// Modify main query
add_action( 'pre_get_posts', 'modify_main_query', 1 );

/**
 * Modify main query.
 *
 * @since 1.0.0
 *
 * @param WP_Query $query Query object.
 */
function modify_main_query( $query ) {
	// Only modify main query
	if ( ! $query->is_main_query() ) {
		return;
	}

	// Only on front page
	if ( ! $query->is_home() ) {
		return;
	}

	// Show only specific category
	$query->set( 'category_name', 'featured' );
	$query->set( 'posts_per_page', 5 );
}
```

### get_posts()

```php
// Simple posts retrieval
$posts = get_posts( array(
	'post_type'      => 'post',
	'posts_per_page' => -1, // All posts
	'orderby'        => 'title',
	'order'          => 'ASC',
) );

foreach ( $posts as $post ) {
	setup_postdata( $post );

	echo esc_html( get_the_title( $post->ID ) );
}

wp_reset_postdata();
```

## Post Types and Taxonomies

### Registering Custom Post Types

```php
add_action( 'init', 'register_custom_post_type' );

/**
 * Register custom post type.
 *
 * @since 1.0.0
 */
function register_custom_post_type() {
	$labels = array(
		'name'               => __( 'Books', 'text-domain' ),
		'singular_name'      => __( 'Book', 'text-domain' ),
		'add_new'            => __( 'Add New', 'text-domain' ),
		'add_new_item'       => __( 'Add New Book', 'text-domain' ),
		'edit_item'          => __( 'Edit Book', 'text-domain' ),
		'new_item'           => __( 'New Book', 'text-domain' ),
		'view_item'          => __( 'View Book', 'text-domain' ),
		'search_items'       => __( 'Search Books', 'text-domain' ),
		'not_found'          => __( 'No books found', 'text-domain' ),
		'not_found_in_trash' => __( 'No books found in Trash', 'text-domain' ),
	);

	$args = array(
		'labels'              => $labels,
		'public'              => true,
		'publicly_queryable'  => true,
		'show_ui'             => true,
		'show_in_menu'        => true,
		'show_in_nav_menus'   => true,
		'show_in_admin_bar'    => true,
		'show_in_rest'        => true, // For Gutenberg
		'query_var'           => true,
		'rewrite'             => array(
			'slug'       => 'books',
			'with_front' => false,
		),
		'capability_type'      => 'post',
		'has_archive'         => true,
		'hierarchical'        => false,
		'menu_position'       => 20,
		'menu_icon'           => 'dashicons-book',
		'supports'            => array(
			'title',
			'editor',
			'thumbnail',
			'excerpt',
			'custom-fields',
		),
		'rest_base'           => 'books',
		'rest_controller_class' => 'WP_REST_Posts_Controller',
	);

	register_post_type( 'book', $args );
}
```

### Registering Custom Taxonomies

```php
add_action( 'init', 'register_custom_taxonomy' );

/**
 * Register custom taxonomy.
 *
 * @since 1.0.0
 */
function register_custom_taxonomy() {
	$labels = array(
		'name'              => __( 'Genres', 'text-domain' ),
		'singular_name'      => __( 'Genre', 'text-domain' ),
		'search_items'      => __( 'Search Genres', 'text-domain' ),
		'all_items'         => __( 'All Genres', 'text-domain' ),
		'parent_item'       => __( 'Parent Genre', 'text-domain' ),
		'parent_item_colon' => __( 'Parent Genre:', 'text-domain' ),
		'edit_item'         => __( 'Edit Genre', 'text-domain' ),
		'update_item'       => __( 'Update Genre', 'text-domain' ),
		'add_new_item'      => __( 'Add New Genre', 'text-domain' ),
		'new_item_name'     => __( 'New Genre Name', 'text-domain' ),
	);

	$args = array(
		'labels'             => $labels,
		'public'             => true,
		'publicly_queryable' => true,
		'hierarchical'       => true,
		'show_ui'           => true,
		'show_in_menu'      => true,
		'show_in_nav_menus' => true,
		'show_in_rest'     => true,
		'show_admin_column' => true,
		'query_var'        => true,
		'rewrite'           => array(
			'slug'         => 'genre',
			'with_front'   => true,
			'hierarchical' => true,
		),
		'capabilities'       => array(
			'manage_terms',
			'edit_terms',
			'delete_terms',
			'assign_terms',
		),
	);

	register_taxonomy( 'genre', array( 'book' ), $args );
}
```

### Meta Boxes

```php
add_action( 'add_meta_boxes', 'add_custom_meta_box' );

/**
 * Add meta box to post type.
 *
 * @since 1.0.0
 */
function add_custom_meta_box() {
	add_meta_box(
		'custom_meta_box',
		__( 'Custom Meta Box', 'text-domain' ),
		'book',
		'normal',
		'default'
	);
}

add_action( 'save_post', 'save_custom_meta_box_data', 10, 2 );

/**
 * Save meta box data.
 *
 * @since 1.0.0
 *
 * @param int     $post_id Post ID.
 * @param WP_Post $post    Post object.
 */
function save_custom_meta_box_data( $post_id, $post ) {
	// Check autosave
	if ( defined( 'DOING_AUTOSAVE' ) && DOING_AUTOSAVE ) {
		return;
	}

	// Check permissions
	if ( ! current_user_can( 'edit_post', $post_id ) ) {
		return;
	}

	// Verify nonce
	if ( ! isset( $_POST['custom_meta_box_nonce'] ) ||
	     ! wp_verify_nonce( $_POST['custom_meta_box_nonce'], 'custom_meta_box' ) ) {
		return;
	}

	// Save data
	if ( isset( $_POST['custom_field'] ) ) {
		$sanitized = sanitize_text_field( $_POST['custom_field'] );
		update_post_meta( $post_id, '_custom_field', $sanitized );
	}
}
```

## Database API

### $wpdb Basics

```php
global $wpdb;

// Select
$results = $wpdb->get_results(
	"SELECT ID, post_title FROM {$wpdb->posts} WHERE post_status = 'publish'"
);

// Get row
$row = $wpdb->get_row(
	"SELECT * FROM {$wpdb->options} WHERE option_name = 'siteurl'"
);

// Get variable
$site_url = $wpdb->get_var(
	"SELECT option_value FROM {$wpdb->options} WHERE option_name = 'siteurl'"
);

// Insert
$wpdb->insert(
	$wpdb->postmeta,
	array(
		'post_id'    => 1,
		'meta_key'   => 'custom_key',
		'meta_value' => 'custom_value',
	),
	array( '%d', '%s', '%s' )
);

// Update
$wpdb->update(
	$wpdb->options,
	array( 'option_value' => 'new_value' ),
	array( 'option_name' => 'siteurl' ),
	array( '%s', '%s' )
);

// Delete
$wpdb->delete(
	$wpdb->postmeta,
	array( 'post_id' => 1, 'meta_key' => 'old_key' ),
	array( '%d', '%s' )
);
```

### $wpdb->prepare()

```php
global $wpdb;

// Single placeholder
$post_id = 1;
$post_title = $wpdb->get_var(
	$wpdb->prepare(
		"SELECT post_title FROM {$wpdb->posts} WHERE ID = %d",
		$post_id
	)
);

// Multiple placeholders
$wpdb->insert(
	$wpdb->postmeta,
	$wpdb->prepare(
		"(post_id, meta_key, meta_value) VALUES (%d, %s, %s)",
		$post_id,
		'my_key',
		'my_value'
	),
	array( '%d', '%s', '%s' )
);

// LIKE queries
$search_term = 'test';
$results = $wpdb->get_results(
	$wpdb->prepare(
		"SELECT * FROM {$wpdb->posts} WHERE post_title LIKE %s",
		'%' . $wpdb->esc_like( $search_term ) . '%'
	)
);
```

### dbDelta (Table Creation)

```php
// Register activation hook
register_activation_hook( __FILE__, 'create_custom_table' );

/**
 * Create custom database table.
 *
 * @since 1.0.0
 */
function create_custom_table() {
	global $wpdb;

	$table_name = $wpdb->prefix . 'custom_data';

	$charset_collate = $wpdb->get_charset_collate();

	$sql = "CREATE TABLE {$table_name} (
		id mediumint(9) NOT NULL AUTO_INCREMENT,
		user_id bigint(20) NOT NULL,
		data text NOT NULL,
		created_at datetime DEFAULT CURRENT_TIMESTAMP NOT NULL,
		PRIMARY KEY  (id),
		KEY user_id (user_id)
	) {$charset_collate};";

	require_once ABSPATH . 'wp-admin/includes/upgrade.php';
	dbDelta( $sql );

	add_option( 'custom_db_version', '1.0' );
}

// Register activation hook
register_deactivation_hook( __FILE__, 'drop_custom_table' );

/**
 * Drop custom database table.
 *
 * @since 1.0.0
 */
function drop_custom_table() {
	global $wpdb;

	$table_name = $wpdb->prefix . 'custom_data';

	$wpdb->query( "DROP TABLE IF EXISTS {$table_name}" );
}
```

## REST API

### Custom Endpoints

```php
add_action( 'rest_api_init', 'register_custom_endpoints' );

/**
 * Register custom REST endpoints.
 *
 * @since 1.0.0
 */
function register_custom_endpoints() {
	register_rest_route(
		'my-plugin/v1',
		'/books',
		array(
			'methods'             => 'GET, POST',
			'callback'            => 'handle_books_request',
			'permission_callback' => 'check_permission',
			'args'                => array(
				'book_id' => array(
					'validate_callback' => 'validate_book_id',
				),
			),
		)
	);
}

/**
 * Handle books request.
 *
 * @since 1.0.0
 *
 * @param WP_REST_Request $request Request object.
 * @return WP_REST_Response|WP_Error Response.
 */
function handle_books_request( $request ) {
	$params = $request->get_params();

	if ( isset( $params['book_id'] ) ) {
		// Get single book
		$book = get_post( $params['book_id'] );

		if ( ! $book || 'book' !== $book->post_type ) {
			return new WP_Error(
				'book_not_found',
				__( 'Book not found', 'text-domain' ),
				array( 'status' => 404 )
			);
		}

		$data = array(
			'id'    => $book->ID,
			'title' => $book->post_title,
			'content' => $book->post_content,
		);

		return rest_ensure_response( $data );
	}

	// Get all books
	$args = array(
		'post_type'      => 'book',
		'posts_per_page' => 10,
	);

	$books = get_posts( $args );
	$data  = array();

	foreach ( $books as $book ) {
		$data[] = array(
			'id'    => $book->ID,
			'title' => $book->post_title,
		);
	}

	return rest_ensure_response( $data );
}

/**
 * Check permissions.
 *
 * @since 1.0.0
 *
 * @return bool True if allowed.
 */
function check_permission() {
	return current_user_can( 'read' );
}

/**
 * Validate book ID.
 *
 * @since 1.0.0
 *
 * @param int $book_id Book ID.
 * @return bool True if valid.
 */
function validate_book_id( $book_id ) {
	return is_numeric( $book_id ) && $book_id > 0;
}
```

### Custom REST Controller

```php
class Books_Controller extends WP_REST_Controller {

	/**
	 * Register routes.
	 *
	 * @since 1.0.0
	 */
	public function register_routes() {
		register_rest_route(
			'my-plugin/v1',
			'/books/(?P<id>[\\d]+)',
			array(
				'methods'             => 'GET',
				'callback'            => array( $this, 'get_item' ),
				'permission_callback' => array( $this, 'get_item_permissions_check' ),
			)
		);
	}

	/**
	 * Get single item.
	 *
	 * @since 1.0.0
	 *
	 * @param WP_REST_Request $request Request.
	 * @return WP_REST_Response|WP_Error Response.
	 */
	public function get_item( $request ) {
		$id = (int) $request['id'];
		$book = get_post( $id );

		if ( ! $book || 'book' !== $book->post_type ) {
			return new WP_Error(
				'book_not_found',
				__( 'Book not found', 'text-domain' ),
				array( 'status' => 404 )
			);
		}

		$data = $this->prepare_item_for_response( $book );
		return rest_ensure_response( $data );
	}

	/**
	 * Check permissions for item.
	 *
	 * @since 1.0.0
	 *
	 * @param WP_REST_Request $request Request.
	 * @return bool|WP_Error True if allowed.
	 */
	public function get_item_permissions_check( $request ) {
		return current_user_can( 'edit_post', $request['id'] );
	}

	/**
	 * Prepare item for response.
	 *
	 * @since 1.0.0
	 *
	 * @param WP_Post $post Post object.
	 * @return array Response data.
	 */
	protected function prepare_item_for_response( $post ) {
		return array(
			'id'       => $post->ID,
			'title'    => $post->post_title,
			'content'  => $post->post_content,
			'excerpt'  => $post->post_excerpt,
			'date'     => $post->post_date,
			'modified' => $post->post_modified,
		);
	}
}

// Register controller
add_action( 'rest_api_init', function() {
	$controller = new Books_Controller();
	$controller->register_routes();
} );
```

## Testing

### PHPUnit Tests

```bash
# Run all tests
phpunit

# Run specific test file
phpunit tests/test-my-file.php

# Run specific test method
phpunit --filter test_my_function

# Run with verbose output
phpunit -v

# Run specific test suite
phpunit tests/unit/ --group my-group
```

### Test Structure

```php
<?php
// tests/test-my-function.php

class Test_My_Function extends WP_UnitTestCase {

	/**
	 * Set up test environment.
	 */
	public function set_up() {
		parent::set_up();
		// Create test posts, users, etc.
		$this->post_id = $this->factory->post->create();
	}

	/**
	 * Clean up test environment.
	 */
	public function tear_down() {
		parent::tear_down();
		// Clean up test data
	}

	/**
	 * Test my_function.
	 */
	public function test_my_function() {
		$result = my_function( $this->post_id );

		$this->assertEquals( 'expected_value', $result );
	}

	/**
	 * Test with factory.
	 */
	public function test_with_factory() {
		$posts = $this->factory->post->create_many( 5 );

		$result = my_function();

		$this->assertCount( 5, $result );
	}

	/**
	 * Test hook.
	 */
	public function test_hook_called() {
		$called = false;

		add_action( 'my_hook', function() use ( &$called ) {
			$called = true;
		} );

		do_action( 'my_hook' );

		$this->assertTrue( $called );
	}
}
```

### wp-cli Scaffold Tests

```bash
# Scaffold plugin with tests
wp scaffold plugin my-plugin

# Scaffold theme with tests
wp scaffold theme my-theme

# Scaffold single test file
wp scaffold test my-test-file
```

### Database Factories

```php
// Using WP_UnitTestCase
class Test_My_Class extends WP_UnitTestCase {

	public function test_create_posts() {
		// Create single post
		$post_id = $this->factory->post->create( array(
			'post_title' => 'Test Post',
			'post_content' => 'Test content',
		) );

		$this->assertIsInt( $post_id );

		// Create multiple posts
		$posts = $this->factory->post->create_many( 10 );

		$this->assertCount( 10, $posts );

		// Create with custom post type
		$book_id = $this->factory->post->create( array(
			'post_type' => 'book',
			'post_title' => 'Test Book',
		) );

		$this->assertInstanceOf( 'WP_Post', get_post( $book_id ) );

		// Create users
		$user_id = $this->factory->user->create( array(
			'role' => 'editor',
		) );

		$this->assertIsInt( $user_id );

		// Create attachments
		$attachment_id = $this->factory->attachment->create( array(
			'file' => DIR_TESTDATA . '/images/test.jpg',
		) );

		$this->assertIsInt( $attachment_id );
	}
}
```

## Trac Workflow

### Finding and Claiming Tickets

1. Browse tickets: https://core.trac.wordpress.org/query?status=!closed
2. Look for "good first bug" or "needs patch" keywords
3. Verify ticket is not assigned
4. Comment "Working on this" to claim

### Creating a Patch

```bash
# Create branch for ticket
git checkout -b 12345-fix-issue

# Make changes
# Edit files, fix bug, etc.

# Commit changes
git commit -m "Fix issue #12345: Description of fix"

# Create patch
git format-patch HEAD~1

# Output: 0001-Fix-issue-12345-Description-of-fix.patch
```

### Diff Format

```diff
--- a/src/wp-includes/my-file.php	(revision 50000)
+++ b/src/wp-includes/my-file.php	(working copy)
@@ -10,7 +10,7 @@
 function my_function( $param ) {
-	return 'old_value';
+	return 'new_value';
 }
```

### Patch Keywords

Trac uses these keywords to track ticket status:

- **has-patch** - Ticket has a patch attached
- **needs-patch** - Patch needed
- **needs-testing** - Ready for testing
- **needs-refresh** - Patch needs refresh for trunk
- **has-unit-tests** - Includes unit tests
- **needs-unit-tests** - Unit tests needed
- **needs-docs** - Documentation needed
- **good-first-bug** - Good for new contributors

### Commit Message Format

```
Bug/Fix/New/Docs #12345: Brief description

Detailed explanation of changes.

More details if needed.

Fixes #12345.
```

### Uploading Patch to Trac

1. Go to ticket page on Trac
2. Click "Attach file"
3. Upload `.patch` file
4. Add comment explaining changes
5. Add appropriate keywords

### Good First Bug Resources

- **Good First Bugs:** https://core.trac.wordpress.org/query?status=!closed&keywords=~good-first-bug
- **Beginner Documentation:** https://make.wordpress.org/docs/handbook/beginner/
- **Handbook:** https://make.wordpress.org/core/handbook/
- **Slack:** #core on Make WordPress Slack

## Common Issues

### Deprecated Functions

```php
// ❌ BAD: Using deprecated function
wp_get_http_headers( 'http://example.com' );

// ✅ GOOD: Use wp_remote_get
$response = wp_remote_get( 'http://example.com' );
$headers = wp_remote_retrieve_headers( $response );

// ❌ BAD: Using deprecated function
esc_url_raw( 'http://example.com' );

// ✅ GOOD: Use esc_url_raw (unchanged but documented properly)
esc_url_raw( 'http://example.com' );
```

### SQL Injection Prevention

```php
// ❌ BAD: Direct variable interpolation
global $wpdb;
$results = $wpdb->get_results(
	"SELECT * FROM {$wpdb->posts} WHERE ID = $user_id"
);

// ✅ GOOD: Use prepare()
global $wpdb;
$results = $wpdb->get_results(
	$wpdb->prepare(
		"SELECT * FROM {$wpdb->posts} WHERE ID = %d",
		$user_id
	)
);
```

### XSS Prevention

```php
// ❌ BAD: Direct output
echo $_POST['title'];

// ✅ GOOD: Escape output
echo esc_html( $_POST['title'] );

// ✅ GOOD: Escape for HTML attributes
echo '<a href="' . esc_url( $url ) . '">Link</a>';

// ✅ GOOD: Escape for JS
echo 'var message = ' . esc_js( $message ) . ';';

// ✅ GOOD: Escape for URL query params
$url = add_query_arg( 'key', 'value', $url );
```

### Nonces for Forms

```php
// Generate nonce
$nonce = wp_create_nonce( 'my_action' );

// Verify nonce
if ( isset( $_POST['my_nonce_field'] ) &&
     wp_verify_nonce( $_POST['my_nonce_field'], 'my_action' ) ) {
	// Process form
}

// In form
echo '<input type="hidden" name="my_nonce_field" value="' . $nonce . '">';
```

### Debugging

```php
// Enable debug mode
define( 'WP_DEBUG', true );

// Enable debug log
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );

// Log to file
error_log( 'Debug message' );

// Or use WordPress debug
if ( WP_DEBUG ) {
	error_log( 'Debug message' );
}
```

## Best Practices

1. **Always follow WordPress Coding Standards**
2. **Use WordPress functions instead of PHP equivalents** (wp_remote_get vs curl)
3. **Sanitize and escape all data**
4. **Use nonces for forms and AJAX**
5. **Always write tests for new features**
6. **Use wp-cli for scaffolding tests**
7. **Read core handbook before contributing**
8. **Test on different PHP versions**
9. **Use Trac keywords appropriately**
10. **Write clear commit messages**

## Resources

- **Developer Resources:** https://developer.wordpress.org/
- **Core Handbook:** https://make.wordpress.org/core/handbook/
- **Trac:** https://core.trac.wordpress.org/
- **Slack:** https://make.wordpress.org/chat/
- **GitHub:** https://github.com/WordPress/wordpress-develop
- **Codex:** https://codex.wordpress.org/ (Archived, use Developer Resources)
- **Plugin Handbook:** https://make.wordpress.org/plugins/handbook/
- **Theme Handbook:** https://make.wordpress.org/themes/handbook/

## Quick Reference

### Common Hooks

```php
add_action( 'init', 'my_init' );                    // Initialize
add_action( 'wp_enqueue_scripts', 'my_scripts' );    // Enqueue scripts
add_action( 'wp_head', 'my_head_content' );        // Add to <head>
add_action( 'wp_footer', 'my_footer_content' );    // Add to footer
add_action( 'save_post', 'my_save_post' );          // Save post
add_action( 'the_content', 'my_filter_content' );    // Filter content
```

### Common Functions

```php
// Posts
get_post( $id );
get_posts( $args );
get_the_title();
the_content();

// Users
get_current_user_id();
wp_get_current_user();
current_user_can( 'capability' );

// Options
get_option( 'option_name' );
update_option( 'option_name', 'value' );
delete_option( 'option_name' );

// Transients (caching)
get_transient( 'key' );
set_transient( 'key', 'value', $expiration );
delete_transient( 'key' );

// URLs
home_url();
site_url();
admin_url();
includes_url();
content_url();

// Helpers
esc_html( $text );
esc_url( $url );
esc_attr( $text );
sanitize_text_field( $text );
sanitize_title( $title );
```
