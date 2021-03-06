# Background

This exercise was done to follow along with the Jane Street tech talk at https://www.youtube.com/watch?v=h_e5pPKI0K4 

# Steps
## Getting build set-up
```
Robs-MacBook-Pro:template robunderwood$ jbuilder build {main.bc.js,index.html,style.css}
Robs-MacBook-Pro:template robunderwood$
```


## Step 2 -- Adding random number list to model

![Image of Random Number Tables](images/step2-adding-randoms-numbers.png)

* Note: Same exactly list of random numbers as were generated in the demo

# Problems encountered
## Library not found
Example:
```
Robs-MacBook-Pro:template robunderwood$ jbuilder build {main.bc.js,index.html,style.css}
File "jbuild", line 6, characters 29-37:
```
Solution: Specify library in jbuild file
Example:
```
(jbuild_version 1)

(executables
 ((names (main))
  (libraries
   (core_kernel async_kernel incr_dom))
  (preprocess (pps (ppx_jane)))
  ))
```

## Couldn't compile after adding line ` Dom_html.window##.location##.search`

Cause: I had not yet added `js_of_ocaml-ppx` to jbuild file
Solution: Add `js_of_ocaml-ppx` to jbuild file
```(jbuild_version 1)

(executables
 ((names (main))
  (libraries
   (core_kernel async_kernel incr_dom))
  (preprocess (pps (ppx_jane js_of_ocaml-ppx)))
  ))
```

## Related: Merlin doesn't "like" the "##" 
See PPX section of this [merlin config file doc](https://github.com/ocaml/merlin/wiki/project-configuration)
Note: Seems like updating jbuild file resolved 

## Compliation errors assocated with `Error err -> (Async_js.log_s ... ` line
Supposed to be:
```Error err -> Async_js.log_s [%message "Failed to parse row num from url" ~_:(err:Error.t)];
None```



Solution: Added parens around expression starting with `Async`:
```Error err -> (Async_js.log_s [%message "Failed to parse row num from url" ~_:(err:Error.t)]);
None```



# Open Questions
* Is latest/current version of dune incompatible with libraries like async?

# Miscelaneous 
* Lots of good incr_dom examples in the [incr_dom github](https://github.com/janestreet/incr_dom/tree/master/example/)