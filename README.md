# SuperRepeater

A powerful repeating container component for Budibase applications that dynamically generates component instances for each data item, supporting all layout modes (Container, Grid, Tabs, SplitView, Field Group) with advanced data binding and flexible configuration options.

## 🚀 Features

### Data-Driven Repetition

- **Dynamic Instance Generation**: Automatically creates component instances for each data item
- **Multiple Data Sources**: Bind to data providers or static arrays
- **Index Tracking**: Access current iteration index and position
- **Item Context**: Each repetition exposes its data item and position

### Layout Modes (Inherited from SuperContainer)

- **Container Mode**: Flexible flexbox layout with alignment and spacing controls
- **Grid Mode**: CSS Grid with configurable columns and rows
- **Tabs Mode**: Tabbed interface with navigation and themes
- **SplitView Mode**: Resizable panels with ratio control
- **Field Group Mode**: Form-style layout with labeled fields

### Advanced Features

- **Repeater Modes**: Off, Data Provider, or Array binding
- **Skeleton Loading**: Optional skeleton states during data loading
- **Collapsible Containers**: Expandable/collapsible master container
- **Context Provision**: Rich context data for child components
- **Flexible Sizing**: Grow/shrink modes with configurable factors

## 🎯 Usage Instructions

### Basic Setup

1. Add the SuperRepeater component to your screen
2. Choose a data binding mode (Data Provider or Array)
3. Configure the data source to determine number of repetitions
4. Set your layout mode and alignment preferences
5. Design the template with child components that will repeat

### Data Binding Configuration

#### Data Provider Mode

- **Data Provider Binding**: Connect to any data source or provider
- **Automatic Repetition**: Creates one instance per data item
- **Context Data**: Each child accesses its specific data item
- **Dynamic Updates**: Automatic updates when data changes

#### Array Mode

- **Array Source**: Bind to template arrays or static arrays
- **Template Arrays**: Support for dynamic array generation
- **Value Access**: Each repetition has access to its array value
- **Index Context**: Current array position and value context

### Layout Mode Selection

#### Container Mode - Most Flexible

- **Direction Control**: Row (horizontal) or Column (vertical) flow
- **Alignment Settings**: Horizontally and vertically position content
- **Spacing Control**: Configurable gaps between repetitions
- **Wrapping Options**: Enable/disable row wrapping for multi-line layouts

#### Grid Mode - Structured Layout

- **Column Configuration**: Adjustable grid column count
- **Row Definition**: Set grid row structure
- **Responsive Adaptation**: Automatic adjustment to available space
- **Span Control**: Column and row spanning for individual items

#### Tabs Mode - Organized Display

- **Tab Themes**: Multiple visual themes (Budibase, Buttons)
- **Size Options**: Small, Medium, Large tab sizing
- **Icon Integration**: Icons for each tab instance
- **Active State**: Control which tab is initially active

#### SplitView Mode - Panel Organization

- **Ratio Setting**: Define initial panel size ratios
- **Resizable Controls**: User-adjustable panel sizes
- **Panel Instances**: Each data item becomes a resizable panel
- **Layout Efficiency**: Parallel panel organization

#### Field Group Mode - Form Layout

- **Label Positioning**: Left-aligned or above field labels
- **Grid Structure**: Multi-column form organization
- **Label Width**: Consistent label sizing for left-aligned layouts
- **Form Synchronization**: Coordinated form field behavior

### Child Component Access

#### Context Variables for Child Components

- **`value`**: The current data item for this repetition
- **`index`**: Zero-based array index of current repetition
- **`_row`**: Complete row object from data source
- **Parent Context**: Access to parent component data

#### Template Binding Examples

```svelte
<!-- Each repetition gets its own data -->
<h2>{value.title}</h2>
<p>Position: {index}</p>

<!-- Child components inherit context -->
<SuperButton text="{value.buttonText}" />
```

## 🔧 Configuration Properties

### Core Repeater Settings

- **Mode**: Container, Grid, Tabs, SplitView, or Field Group layout
- **Repeater Mode**: Off, Data Provider, or Array binding
- **Child Mode**: Container Item, SplitView Item, Tabs Item, Grid Item, or Field Group Item

### Data Binding Options

- **Data Provider**: Bind to any data source or provider
- **Source Array**: Bind to template arrays or static arrays
- **Skeleton**: Enable loading skeleton states
- **Collapsible**: Allow container collapse/expand

### Layout-Specific Configuration

#### Container Mode Settings

- **Direction**: Row or column layout flow
- **Horizontal Alignment**: Left, center, right, space-between, space-around, stretch
- **Vertical Alignment**: Top, middle, bottom, space-between, space-around, stretch
- **Gap**: None (0), Tiny (0.25), Small (0.5), Medium (1), Large (1.5)
- **Wrap**: Enable/disable item wrapping

#### Grid Mode Settings

- **Grid Columns**: Number of columns (default: 3)
- **Grid Rows**: Number of rows (default: 3)
- **Column Span**: Individual item column spans (default: 1)
- **Row Span**: Individual item row spans (default: 1)

#### Tabs Mode Settings

- **Theme**: Budibase or Buttons theme
- **Size**: Small (S), Medium (M), Large (L)
- **Text Alignment**: Left, Center, Right
- **Active Tab**: Control which tab is initial
- **Emphasized**: Enable emphasized tab styles
- **Icons Only**: Display tab icons only

#### Collapsible Settings

- **Collapsed**: Start in collapsed state
- **Panel Title**: Title when expanded
- **Collapse Title**: Title when collapsed
- **Collapse Icon**: Icon for collapsed state
- **Collapse Size**: Dimensions of collapsed panel (default: 3rem)
- **Collapse Side**: Left, Top, Right, or Bottom positioning

#### Field Group Settings

- **Label Position**: Left or Above field labels
- **Label Width**: Width for left-aligned labels (default: 6rem)
- **Grid Columns**: Number of form columns (default: 3)
- **Disabled**: Enable/disable entire field group

### Flex Sizing Options

- **Flex Mode**: Shrinking or growing flexible sizing
- **Flex Factor**: Configuration factor for flexible sizing (default: 1)

## 📋 Usage Examples

### Dashboard Card Grid

Create a responsive dashboard with data-driven cards:

- Mode: Grid
- Data Provider: `dashboardCards` data source
- Grid Columns: 4
- Each repetition generates a card with `value.title`, `value.metric`, etc.

### Product Gallery - Container Mode

Build a product gallery with organized layout:

- Mode: Container
- Direction: Row
- Wrap: Enabled
- Data Provider: `products` table
- Each repetition shows product image, title, price

### Field Collection - Tabs Mode

Organize dynamic form fields in tabs:

- Mode: Tabs
- Data Provider: `dynamicSections` data source
- Theme: Budibase
- Each tab displays `value.fieldName` and corresponding form fields

### Document Viewer - SplitView Mode

Create a document viewer with navigation:

- Mode: SplitView
- Data Provider: `documents` data source
- Primary panel: Document list (`value.filename`)
- Secondary panel: Document content (`value.content`)

### Form Field Generator - Field Group Mode

Generate dynamic form fields based on configuration:

- Mode: Field Group
- Source Array: `formFields` array
- Label Position: Above
- Grid Columns: 3
- Each repetition generates appropriate form field

## 🎨 Styling & Theming

### Container Styling

- **Padding Control**: Adjust container padding and margins
- **Border Styling**: Configure border styles and radius
- **Background Options**: Solid colors, gradients, or transparency
- **Hover Effects**: Interactive feedback for container interactions

### Responsive Design

- **Layout Adaptation**: Automatic mode switching for different screen sizes
- **Content Reflow**: Efficient content reorganization at breakpoints
- **Touch Optimization**: Mobile-friendly interaction patterns
- **Flexible Scaling**: Component scaling with available space

### Performance Optimization

- **Lazy Rendering**: Efficient rendering for large data sets
- **Virtualization**: Memory-efficient large list handling
- **Update Optimization**: Smart re-rendering only when necessary
- **Data Caching**: Intelligent data source caching strategies

## 🔗 Integration & Context

### Child Component Context Access

#### Data Access Patterns

```svelte
<!-- Access current data item -->
<h3>{value.itemName}</h3>

<!-- Use iteration index -->
<span>Item #{index + 1} of {repetitions.length}</span>

<!-- Access full data object -->
<input bind:value={value.fieldValue} />

<!-- Access parent context -->
<strong>Section: {$parent.selectedSection}</strong>
```

#### Template Context Integration

- **Dynamic Templates**: Use data to determine child component appearance
- **Conditional Rendering**: Show/hide content based on data properties
- **Interactive Elements**: Components can interact with step-specific data
- **State Synchronization**: Parent-child data synchronization

### Nested Repeater Support

- **Hierarchical Data**: Handle nested array structures
- **Parent Context**: Access parent repeater context from children
- **Cross-Repeater Communication**: Coordinate between multiple repeaters
- **Local State Management**: Maintain state within each repetition

## 📱 Advanced Features

### Skeleton Loading States

- **Loading Indicators**: Visual loading states during data fetching
- **Progressive Loading**: Show content as data becomes available
- **Placeholder Content**: Meaningful placeholders during loading
- **Smooth Transitions**: Consistent loading experiences

### Collapsible Functionality

- **State Persistence**: Remember collapse/expand states
- **Animation Timing**: Smooth expand/collapse transitions
- **Accessibility**: Screen reader support for collapsible content
- **Event Signaling**: Events when collapse state changes

### Performance Considerations

- **Virtual Scrolling**: Handle thousands of items efficiently
- **Incremental Rendering**: Render only visible items
- **Memory Management**: Efficient resource allocation for large datasets
- **Caching Strategies**: Intelligent data and component caching

## 🐛 Troubleshooting

### Data Binding Issues

- **Data Source Configuration**: Verify data source connectivity and permissions
- **Binding Syntax**: Ensure correct template binding patterns
- **Data Structure**: Confirm data matches expected format
- **Update Timing**: Check data provider refresh settings

### Layout Problems

- **Responsive Behavior**: Test layout at different screen sizes
- **Flex Configuration**: Verify flex factor and sizing settings
- **Grid Alignment**: Check column/row span configurations
- **Container Overflow**: Address content overflow issues

### Performance Issues

- **Large Datasets**: Implement pagination or virtualization
- **Complex Templates**: Simplify or optimize nested component structures
- **Update Frequency**: Reduce unnecessary re-renders through optimization
- **Memory Leaks**: Monitor component lifecycle and data cleanup

Find out more about developing [Custom Plugins](https://docs.budibase.com/docs/custom-plugin) and [Budibase](https://github.com/Budibase/budibase).
