# Useful console snippets for Stellaris
## Hyperlane modification
You can mark multiple systems, in this case all of them will get hyperlane created/deleted to target system.

1. Select any object in source system and add `hyperlane_manipulator_marker` flag to it:
    ```
    effect solar_system = {
      set_star_flag = hyperlane_manipulator_marker
    }
    ```
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

## Wormhole creation
Trying to create wormhole in system that already has wormhole will break existing connections, so commans will prevent creation of another wormhole pair if either source or target system already has any.

1. Select any object in source system and add `wormhole_manipulator_marker` flag to it:
    ```
    effect solar_system = {
      set_star_flag = wormhole_manipulator_marker  
    }
    ```
1. Select any object in target system and create wormhole pair:
    ```
    effect solar_system = {
      if = {
        limit = {
          NOT = {
            has_natural_wormhole = yes
          }
        }
        random_system = {
          limit = {
            has_star_flag = wormhole_manipulator_marker
            NOT = {
              has_natural_wormhole = yes
            }
          }
          prev = {
            spawn_natural_wormhole = {
              bypass_type = wormhole
              random_pos = yes
              orbit_angle = 360
            }
          }
          spawn_natural_wormhole = {
            bypass_type = wormhole
            random_pos = yes
            orbit_angle = 360
          }
          link_wormholes = prev
        }
      }

      every_system = {
        limit = {
          has_star_flag = wormhole_manipulator_marker
        }
        remove_star_flag = wormhole_manipulator_marker
      }
    }
    ```
1. Clear `wormhole_manipulator_marker` marker flag from all systems:
    ```
    effect every_system = {
      limit = {
        has_star_flag = wormhole_manipulator_marker
      }
      remove_star_flag = wormhole_manipulator_marker
    }
    ```
