# State Pattern
## Overview
State pattern is a **behavioural pattern** that lets an object **alter its behaviour** when its **internal state changes**. It appears as if the object has **changed its class**.

It essentially **extends strategy pattern**, but the strategies are **aware of each other and the containing context**. The strategies are able call methods on its containing context as well as **change the state of the context**.

## Advantages
- Allows a context to **change behaviour** as its state changes.
- Encapsulating each state into a class **localises any changes**.
- Simplifies the code of the context by **eliminating state conditionals**.

## Disadvantages
- Introduces a lot of classes. This can be overkill if the state machine has only a few states.

## Applicability
- When you have an object that behaves **differently depending on its current state**, the number of states is large, and the state-specific code changes frequently.
- When you have a class polluted with **massive behavioural conditionals** depending on the field values.
- When you have a lot of **duplicate code across similar states** and transitions of a condition-based state machine.

## Example
```java
public class AudioPlayer {
    State state;

    public AudioPlayer() {
        this.state = new ReadyState(this);
    }

    public void changeState(State state) {
        this.state = state;
    }

    // Methods are delegated to the state object.
    public void clickLock() {
        state.clickLock();
    }
    public void clickPlay() {
        state.clickPlay();
    }
    public void clickNext() {
        state.clickNext();
    }
    public void clickPrev() {
        state.clickPrev();
    }

    // A state object may call some service methods on this context object.
    public void startPlayback();
    public void stopPlayback();
    public void nextSong();
    public void prevSong();
    public void fastForward(Integer time);
    public void rewind(Integer time);
}

public abstract class State {
    AudioPlayer player;

    public State(AudioPlayer player) {
        this.player = player;
    }

    public abstract void clickLock();
    public abstract void clickPlay();
    public abstract void clickNext();
    public abstract void clickPrev();
}

public class LockedState extends State {
    public void clickLock() {
        if (player.playing) player.changeState(new PlayingState(player));
        else player.changeState(new ReadyState(player));
    }

    public void clickPlay() {
        // do nothing
    }

    public void clickNext() {
        // do nothing
    }

    public clickPrev() {
        // do nothing
    }
}

public class ReadyState extends State {
    public void clickLock() {
        player.changeState(new LockedState(player));
    }

    public void clickPlay() {
        player.startPlayback();
        player.changeState(new PlayingState(player));
    }

    public void clickNext() {
        player.nextSong();
    }

    public clickPrev() {
        player.prevSong();
    }
}
}

public class PlayingState extends State {
    public void clickLock() {
        player.changeState(new LockedState(player));
    }

    public void clickPlay() {
        player.stopPlayback();
        player.changeState(new ReadyState(player));
    }

    public void clickNext() {
        if (event.doubleClick) player.nextSong();
        else player.fastForward(5);
    }

    public clickPrev() {
        if (event.doubleClick) player.prevSong();
        else player.rewind(5);
    }
}
```

State transitions can be controlled by the state classes or by the context class. Here, it is handled by the state classes.