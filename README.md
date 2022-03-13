# Useful console snippets for Stellaris
## Hyperlane modification
1. Select any object in source system and add `hyperlane_manipulator_marker` flag to it:
    ```
    effect solar_system = {
      text = "Marking..."
      set_star_flag = hyperlane_manipulator_marker
    }
    ```
    You can mark multiple systems, in this case all of them will get hyperlane created/deleted to target system.
1. Select any object in target system
    1. Create new hyperlane between target system and source system(s):
        ```
        effect solar_system = {
          every_system = { 
            limit = {
              has_star_flag = hyperlane_manipulator_marker 
            }
            if = {
              limit = {
                NOT = {
                  has_hyperlane_to = prev
                }
              }
              add_hyperlane = {
                from = this
                to = prev
              }
            }
            remove_star_flag = hyperlane_manipulator_marker
          }
        }
        ```
    1. Remove existing hyperlane between target system and source system(s):
        ```
        effect solar_system = {
          every_system = { 
            limit = {
              has_star_flag = hyperlane_manipulator_marker 
            }
            if = {
              limit = {
                has_hyperlane_to = prev
              }
              remove_hyperlane = {
                from = this
                to = prev
              }
            }
            remove_star_flag = hyperlane_manipulator_marker
          }
        }
        ```
1. Clear `hyperlane_manipulator_marker` marker flag from all systems:
    ```
    effect every_system = {
      limit = {
        has_star_flag = hyperlane_manipulator_marker
      }
      remove_star_flag = hyperlane_manipulator_marker
    }
    ```
