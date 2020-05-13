## Creating a Component

Generate the scaffold:

`php artisan make:component SomeComponent`

Two files will be generated:

```
app/View/Components/SomeComponent.php
resources/views/components/some-component.blade.php
```
### Logic

Default file from scaffold:

```php
<?php

namespace App\View\Components;

use Illuminate\View\Component;

class SomeComponent extends Component
{
    /**
     * Create a new component instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Get the view / contents that represent the component.
     *
     * @return \Illuminate\View\View|string
     */
    public function render()
    {
        return view('components.some-component');
    }
}
```

### Default Template

The template from scaffolding:

```php
<div>
    <!-- Smile, breathe, and go slowly. - Thich Nhat Hanh -->
</div>
```

### Displaying in a View

To use the component in a Blade template:

`<x-some-component/>`

With a parameter:

`<x-some-component some_param="this"/>`

If you need to use a PHP variable as the parameter value:

`<x-some-component :some_param="$this->value"/>`