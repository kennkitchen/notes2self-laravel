## Adding CRUD functionality to an app

### Create the Model
Assuming we have a table called "contacts" in our database already:

`php artisan make:model Contact`

This will create a skeleton model:

app/Contact.php
```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Contact extends Model
{
    //
}
```

You will want to add the following to the model:

```php
protected $fillable = [
        'name', 
        'email',
        'website'
    ];
```

(The field names are examples of the data you'll be using on your forms.)

### Create the Controller

Scaffold the controller with:

`php artisan make:controller ContactController --resource`

By designating this as a resource controller, the result file with have stub methods for CRUD functionality:

app/Http/Controllers/ContactController
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ContactController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        //
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        //
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        //
    }
}
```

For each method, using the example fields from above, this is how the code would fill in:

app/Http/Controllers/ContactController
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Contact;

class ContactController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        $contacts = Contact::all();

        return view('contacts.index', compact('contacts'));
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        return view('contacts.create');
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $request->validate([
            'name' => 'required',
            'email' => 'required'
        ]);

        $contact = new Contact([
            'name' => $request->get('name'),
            'email' => $request->get('email'),
            'website' => $request->get('website')
        ]);
        $contact->save();
        return redirect('/contacts')->with('success', 'Contact saved!');
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        $contact = Contact::find($id);

        return view('contacts.show', compact('contact'));
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        $contact = Contact::find($id);

        return view('contacts.edit', compact('contact'));
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        $request->validate([
            'name' => 'required',
            'email' => 'required',
        ]);

        $contact = Contact::find($id);

        $contact->name = $request->get('name');
        $contact->email = $request->get('email');
        $contact->website = $request->get('website');

        $contact->save();

        return redirect('/contacts')->with('success', 'Contact updated!');
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        $contact = Contact::find($id);
        $contact->delete();

        return redirect('/contacts')->with('success', 'Contact deleted!');
    }
}
```

### Create templates

Under resources/views, create a folder called "contacts". We need four template files:

* create.blade.php
* edit.blade.php
* index.blade.php
* show.blade.php

resources/views/contacts/create.blade.php
```php
@extends('layouts.app')

@section('content')
    <div class="row">
        <div class="col-sm-8 offset-sm-2">
            <h2>Add a Contact</h2>
            <div>
                @if ($errors->any())
                    <div class="alert alert-danger">
                        <ul>
                            @foreach ($errors->all() as $error)
                                <li>{{ $error }}</li>
                            @endforeach
                        </ul>
                    </div><br />
                @endif
                <form method="post" action="{{ route('contacts.store') }}">
                    @csrf
                    <div class="form-group">
                        <label for="name">Name:</label>
                        <input type="text" class="form-control" name="name"/>
                    </div>
                    <div class="form-group">
                        <label for="email">Email Address:</label>
                        <input type="text" class="form-control" name="email"/>
                    </div>
                    <div class="form-group">
                        <label for="website">Website:</label>
                        <input type="text" class="form-control" name="website"/>
                    </div>
                    <button type="submit" class="btn btn-primary">Add Contact</button>
                </form>
            </div>
        </div>
    </div>
@endsection
```

resources/views/contacts/edit.blade.php
```php
@extends('layouts.app')

@section('content')
    <div class="row">
        <div class="col-sm-8 offset-sm-2">
            <h2>Update a Contact</h2>

            @if ($errors->any())
                <div class="alert alert-danger">
                    <ul>
                        @foreach ($errors->all() as $error)
                            <li>{{ $error }}</li>
                        @endforeach
                    </ul>
                </div>
                <br />
            @endif
            <form method="post" action="{{ route('contacts.update', $contact->id) }}">
                @method('PATCH')
                @csrf
                <div class="form-group">
                    <label for="name">Name:</label>
                    <input type="text" class="form-control" name="name" value="{{ $contact->name }}" />
                </div>
                <div class="form-group">
                    <label for="email">Email Address:</label>
                    <input type="text" class="form-control" email="email" value="{{ $contact->email }}" />
                </div>
                <div class="form-group">
                    <label for="website">Website:</label>
                    <input type="text" class="form-control" website="website" value="{{ $contact->website }}" />
                </div>
                <button type="submit" class="btn btn-primary">Update Contact</button>
            </form>
        </div>
    </div>
@endsection
```

resources/views/contacts/index.blade.php
```php
@extends('layouts.app')

@section('content')
    <div class="row container-fluid">
        <div class="col-sm-12">
            <table id="default" class="table table-hover" style="width:100%">
                <thead>
                <tr>
                    <td>ID</td>
                    <td>Name</td>
                    <td>Email</td>
                    <td>Website</td>
                    <td>Created</td>
                    <td>Modified</td>
                </tr>
                </thead>
                <tbody>
                @foreach($contacts as $contact)
                    <tr>
                        <td>{{$contact->id}}</td>
                        <td>{{$contact->name}}</td>
                        <td>{{$contact->email}}</td>
                        <td>{{$contact->website}}</td>
                        <td>{{$contact->created_at}}</td>
                        <td>{{$contact->updated_at}}</td>
                    </tr>
                @endforeach
                </tbody>
            </table>
        </div>
    </div>
@endsection
```

resources/views/contacts/show.blade.php
```php
@extends('layouts.app')

@section('content')
    <div class="row">
        <div class="col-sm-8 offset-sm-2">
            <h2>{{ $contact->name }}</h2>
            <div class="row">
                <div class="col-sm">
                    <p><strong>Email:</strong> {{ $contact->email }}</p>
                    <p><strong>Website:</strong> {{ $contact->website }}</p>
                    <p><strong>Created:</strong> {{ $contact->created_at }}</p>
                    <p><strong>Updated:</strong> {{ $contact->updated_at }}</p>
                </div>
            </div>
        </div>
    </div>
@endsection
```

### Adding Routes

Finally, update the routes file (`routes/web.php`) with a resource route entry:

`Route::resource('contacts', 'ContactController');`
