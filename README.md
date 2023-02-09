# Leptos How To

A How To collection for common [leptos](https://github.com/leptos-rs/leptos) patterns.

# How To:

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

## Log an `&str` in the Browser or on the Server
* `console_log(&str)` https://docs.rs/leptos_dom/latest/leptos_dom/fn.console_log.html
* `console_warn(&str)` https://docs.rs/leptos_dom/latest/leptos_dom/fn.console_warn.html
* `console_error(&str)` https://docs.rs/leptos_dom/latest/leptos_dom/fn.console_error.html



