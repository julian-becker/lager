
add_custom_target(examples COMMENT "Build all examples")
add_dependencies(check examples)

# std  examples
# ====================

add_executable(counter-std EXCLUDE_FROM_ALL
  counter/std/main.cpp)
target_link_libraries(counter-std lager)
add_dependencies(examples counter-std)

# ncurses examples
# ================

set(CMAKE_CURSES_NEED_WIDE true)

find_package(Curses)

set(counter_ncurses_sources
  counter/ncurses/main.cpp
  counter/ncurses/terminal.cpp)
set(counter_ncurses_include_directories
  ${CURSES_INCLUDE_DIR})
set(counter_ncurses_link_libraries
  lager-debugger
  ${CURSES_LIBRARIES})

add_executable(counter-ncurses EXCLUDE_FROM_ALL ${counter_ncurses_sources})
target_include_directories(counter-ncurses SYSTEM PUBLIC ${counter_ncurses_include_directories})
target_link_libraries(counter-ncurses ${counter_ncurses_link_libraries})
target_compile_definitions(counter-ncurses PUBLIC DEBUGGER)
add_dependencies(examples counter-ncurses)

add_executable(counter-ncurses-tree EXCLUDE_FROM_ALL ${counter_ncurses_sources})
target_include_directories(counter-ncurses-tree SYSTEM PUBLIC ${counter_ncurses_include_directories})
target_link_libraries(counter-ncurses-tree ${counter_ncurses_link_libraries})
target_compile_definitions(counter-ncurses-tree PUBLIC TREE_DEBUGGER)
add_dependencies(examples counter-ncurses-tree)

add_executable(counter-ncurses-meta EXCLUDE_FROM_ALL ${counter_ncurses_sources})
target_include_directories(counter-ncurses-meta SYSTEM PUBLIC ${counter_ncurses_include_directories})
target_link_libraries(counter-ncurses-meta ${counter_ncurses_link_libraries})
target_compile_definitions(counter-ncurses-meta PUBLIC DEBUGGER META_DEBUGGER)
add_dependencies(examples counter-ncurses-meta)

# sdl examples
# ===========

find_package(SDL2)
find_package(SDL2_ttf)

if (SDL2_FOUND AND SDL2_ttf_FOUND)
  add_executable(counter-sdl2 EXCLUDE_FROM_ALL
    counter/sdl2/main.cpp)
  target_include_directories(counter-sdl2 SYSTEM PUBLIC
    ${SDL2_INCLUDE_DIR}
    ${SDL2_TTF_INCLUDE_DIR})
  target_link_libraries(counter-sdl2
    lager-debugger
    ${SDL2_LIBRARY}
    ${SDL2_TTF_LIBRARIES})
  add_dependencies(examples counter-sdl2)

  add_executable(autopong EXCLUDE_FROM_ALL
    autopong/main.cpp)
  target_include_directories(autopong SYSTEM PUBLIC
    ${SDL2_INCLUDE_DIR}
    ${SDL2_TTF_INCLUDE_DIR})
  target_link_libraries(autopong
    lager-debugger
    ${SDL2_LIBRARY}
    ${SDL2_TTF_LIBRARIES})
  add_dependencies(examples autopong)

  add_executable(autopong-debug EXCLUDE_FROM_ALL
    autopong/main.cpp)
  target_compile_definitions(autopong-debug PUBLIC DEBUGGER)
  target_include_directories(autopong-debug SYSTEM PUBLIC
    ${SDL2_INCLUDE_DIR}
    ${SDL2_TTF_INCLUDE_DIR})
  target_link_libraries(autopong-debug
    lager-debugger
    ${SDL2_LIBRARY}
    ${SDL2_TTF_LIBRARIES})
  add_dependencies(examples autopong-debug)
else()
  message(STATUS "Disabling SDL based examples")
endif()
