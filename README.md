# MusicPlaylist Component

A Java software component for managing a music playlist, built following the OSU Software Sequence discipline. It extends `Standard` and provides both a kernel interface and a richer secondary interface for common playlist operations.

---

## Overview

`MusicPlaylist` models a sequenced list of songs with operations for adding, removing, navigating, sorting, and shuffling tracks. It is designed to be used as a reusable component in any Java project that needs playlist functionality.

The component is structured in two layers:

- **`MusicPlaylistKernel`** — core (primitive) operations that form the foundation
- **`MusicPlaylist`** — secondary operations built on top of the kernel

---

## Component Hierarchy

```
Standard<MusicPlaylist>
        │
        ▼
MusicPlaylistKernel
        │
        ▼
   MusicPlaylist
        │
        ▼
MusicPlaylistSecondary  (abstract class)
        │
        ▼
  MusicPlaylist1L       (concrete implementation)
```

---

## Kernel Methods

Defined in `MusicPlaylistKernel.java`. These are the primitive operations — everything else is built from these.

| Method | Description |
|---|---|
| `void addSong(String song)` | Adds a song to the end of the playlist |
| `String removeFirstSong()` | Removes and returns the first song |
| `void skipSong()` | Moves the first song to the end of the playlist |
| `int numberOfSongs()` | Returns the total number of songs |

---

## Secondary Methods

Defined in `MusicPlaylist.java`. These are higher-level operations layered on top of the kernel.

| Method | Description |
|---|---|
| `boolean hasNoSongs()` | Returns `true` if the playlist is empty |
| `void shufflePlaylist()` | Randomly reorders the songs |
| `void sortPlaylist(Comparator<String> order)` | Sorts songs by a given comparator (e.g., by title or artist) |
| `String firstSong()` | Returns the first song without removing it |
| `void playSong(String songTitle)` | Brings a specific song to the front of the playlist |

---

## Usage Example

```java
// Create a new playlist
MusicPlaylist playlist = new MusicPlaylist1L();

// Add songs
playlist.addSong("Bohemian Rhapsody");
playlist.addSong("Hotel California");
playlist.addSong("Stairway to Heaven");

// Check the first song
System.out.println(playlist.firstSong()); // Bohemian Rhapsody

// Skip the current song
playlist.skipSong();
System.out.println(playlist.firstSong()); // Hotel California

// Bring a specific song to the front
playlist.playSong("Stairway to Heaven");
System.out.println(playlist.firstSong()); // Stairway to Heaven

// Shuffle the playlist
playlist.shufflePlaylist();

// Sort alphabetically by title
playlist.sortPlaylist(Comparator.naturalOrder());

// Remove the first song
String removed = playlist.removeFirstSong();
System.out.println(removed + " has been played and removed.");
```

---

## Design Contracts

This component follows **design-by-contract** principles. Each method has defined preconditions (`@requires`) and postconditions (`@ensures`).

Key preconditions to be aware of:

- `removeFirstSong()`, `skipSong()`, `firstSong()`, and `playSong()` all **require** the playlist to have at least one song.
- `playSong(songTitle)` **requires** a song with that exact title to already be in the playlist.

Violating a precondition results in undefined behavior — always check `hasNoSongs()` or `numberOfSongs()` before calling these methods when the playlist state is uncertain.

---

## File Structure

```
src/
├── MusicPlaylistKernel.java       # Kernel interface
├── MusicPlaylist.java             # Secondary (enhanced) interface
├── MusicPlaylistSecondary.java    # Abstract class with secondary implementations
└── MusicPlaylist1L.java           # Concrete kernel implementation
```

---

## Dependencies

This component depends on the **OSU CSE Components** library, which provides the `Standard` interface and related infrastructure.

- `components.standard.Standard`

Make sure the OSU components library is on your classpath before compiling.

---

## Author

- **Berenice Araiza Sierra** — OSU Portfolio Project
- **Jeremy Grifski** - Template Author
