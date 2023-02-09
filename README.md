# Leptos How To

A How To collection for common [leptos](https://github.com/leptos-rs/leptos) patterns.


# Reactive Templating

## if/else render 2 different views based on a reactive variable

`<Show when fallback>` https://docs.rs/leptos/latest/leptos/fn.Show.html

 - `when` is the condition function, must return a `bool`.
 - `fallback` is the else/false view to render.
 - the element's children are the if/true view to render.
 
Example in docs.

## Conditionally render a single view

Use `<Show>` as above but with a fallback that renders an empty view.

Example adapted from the docs for `<Show>`:

```rust
let (value, set_value) = create_signal(cx, 0);

view! { cx,
  <Show
    when=move || value() == 5
    fallback=|cx| view! { cx, {} }
  >
    "Equals 5"
  </Show>
}
```
# Navigation

## Navigate the browser in javascript
`use_navigate(Scope)` returns a function that can be used to navigate the browser.

Example:
```rust
let nav = use_navigate(cx);
let _ = nav("/somewhere-else", Default::default());
```

The second argument is a `NavigateOptions` struct that can be used to effect how the navigation takes place. https://docs.rs/leptos_router/latest/leptos_router/struct.NavigateOptions.html

_Note: This of course does not work if javascript is turned off as it happens only in the browser._

For example, this does nothing:
```rust
if is_server() {
    let nav = use_navigate(cx);
    let _ = nav("/", Default::default());
}
```

## Redirect on the server
This is server framework dependent.

### With Actix

use `leptos_actix::redirect()` https://docs.rs/leptos_actix/0.1.3/leptos_actix/fn.redirect.html

Example:
```rust
leptos_actix::redirect(cx, "/somewhere-else");
```

## Changing the response
This is server framework dependent.

### With Actix

use `use_context::<leptos_actix::ResponseOptions>` 
* `use_context` https://docs.rs/leptos/latest/leptos/fn.use_context.html
* `leptos_actix::ResponseOptions` https://docs.rs/leptos_actix/latest/leptos_actix/struct.ResponseOptions.html

Example:
```rust
// leptos_actix's implementation of `redirect()`
pub async fn redirect(cx: leptos::Scope, path: &str) {
    let response_options = use_context::<ResponseOptions>(cx).unwrap();
    response_options.set_status(StatusCode::FOUND).await;
    response_options
        .insert_header(
            header::LOCATION,
            header::HeaderValue::from_str(path).expect("Failed to create HeaderValue"),
        )
        .await;
}
```

# Misc

## Log things in the Browser or on the Server
* `log!()` https://docs.rs/leptos_dom/latest/leptos_dom/macro.log.html
* `warn!()` https://docs.rs/leptos_dom/latest/leptos_dom/macro.warn.html
* `error!()` https://docs.rs/leptos_dom/latest/leptos_dom/macro.error.html

example: 
```rust
log!("Anything that println!() can handle");
log!("{:?}", Some("thing that implents Debug"));
```




