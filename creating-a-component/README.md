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

```html
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

### Example

Here's an example of a component to produce a dropdown list with an existing value (if a value is present):

app/View/Components/YesNoOther.php
```php
<?php

namespace App\View\Components;

use Illuminate\View\Component;

/**
 * Class YesNoOther
 * @package App\View\Components
 */
class YesNoOther extends Component
{
    /**
     * @var
     */
    public $current_value;

    /**
     * Create a new component instance.
     *
     * @return void
     */
    public function __construct($currentValue)
    {
        $this->currentValue = $currentValue;
    }

    /**
     * Get the view / contents that represent the component.
     *
     * @return \Illuminate\View\View|string
     */
    public function render() {
        return view('components.yes-no-other');
    }

    /**
     * @param $option
     * @return bool
     */
    public function isSelected($option) {
        return $option === $this->currentValue;
    }
}
```

resources/views/components/yes-no-other.blade.php
```php
<option value=""></option>
<option {{ $isSelected("Yes") ? 'selected="selected"' : '' }} value="Yes">Yes</option>
<option {{ $isSelected("No") ? 'selected="selected"' : '' }} value="No">No</option>
<option {{ $isSelected("Other") ? 'selected="selected"' : '' }} value="Other">Other</option>
```

To use in a Blade template, such as a create template where there is no pre-existing value:

`<x-yes-no-other current_value=""/>`

In an edit template, for example, where a value may already exist:

`<x-yes-no-other :current_value="$person->employed"/>`
