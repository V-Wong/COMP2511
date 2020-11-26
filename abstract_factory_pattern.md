# Abstract Factory Pattern
## Overview
Abstract factory is a **creational pattern** that lets you produce **familes of related objects** without specifying their concrete classes.

## Advantages
- Ensures that all products from the same factory are **compatible with each other**.
- Avoid **coupling** between concrete products and client code.
- Satisfies **single responsibility principle** as product creation code is extracted into one place.
- Satisfies **open/closed principle** as new variants of products can be introduced without breaking existing client code.

## Disadvantages
- Code becomes more **complex** with introduction of interfaces and classes.

## Applicability
- When the code needs to work with **various families of related products**, but we don't want it to depend on the concrete classes of these products - these might be **unknown beforehand** or we want to allow for **future extensibility**. Objects created via a factory are designed to be **compatible with other objects** created by the same factory.

## Example
```java
interface Button {
    public void setLabel(String labelText);
    public void click();
}

interface CheckBox {	
	public void setText(String text);
	public String getText();

	public void setStatus(boolean status);
	public boolean getStatus();	
}

// Abstract factory interfaces declares a set of methods that
// return different abstract objects. These products together
// form a product family.
interface GUIFactory {
    Button getButton(); 
    CheckBox getCheckBox();
}

class ButtonLinux implements Button {
    private String labelText = "";

	@Override
	public void setLabel(String labelText) {
		this.labelText = labelText;
		System.out.println("ButtonLinux: Setting label " + labelText);
	}

	@Override
	public void click() {
		System.out.println("ButtonLinux: clicking button with label " + this.labelText);	
	}
}

class ButtonWin implements Button {
    private String labelText = "";

	@Override
	public void setLabel(String labelText) {
		this.labelText = labelText;
		System.out.println("ButtonWin: Setting label " + labelText);
		
    }

	@Override
	public void click() {
		System.out.println("ButtonWin: clicking button with label " + this.labelText);
	}
}

class CheckBoxLinux implements CheckBox {
	private String text = "";
	private boolean status = false;
	
	@Override
	public void setText(String text) {
		this.text = text;
		System.out.println("CheckBoxLinux: Setting Text " + this.text);

	}

	@Override
	public String getText() {
		return this.text;
	}	
	
	@Override
	public void setStatus(boolean status) {
		this.status = status;
		System.out.println("CheckBoxLinux: setStatus status " + this.status);

	}

	@Override
	public boolean getStatus() {
		return this.status;
	}
}

class CheckBoxWin implements CheckBox {
	private String text = "";
	private boolean status = false;
	
	@Override
	public void setText(String text) {
		this.text = text;
		System.out.println("CheckBoxWin: Setting Text " + this.text);
	}

	@Override
	public String getText() {
		return this.text;
	}	
	
	@Override
	public void setStatus(boolean status) {
		this.status = status;
		System.out.println("CheckBoxWin: setStatus status " + this.status);
	}

	@Override
	public boolean getStatus() {
		return this.status;
	}
}

// A Linux Factory can produce a Linux button and a Linux checkbox.
class GUIFactoryLinux implements GUIFactory {
	@Override
	public Button getButton() {
		return new ButtonLinux();
	}

	@Override
	public CheckBox getCheckBox() {
		return new CheckBoxLinux();
	}
}

// A Windows Factory can produce a Windows button and a Windows checkbox.
class GUIFactoryWin implements GUIFactory {
	@Override
	public Button getButton() {
		return new ButtonWin();
	}

	@Override
	public CheckBox getCheckBox() {
		return new CheckBoxWin();
	}	
}

public class Test {
	public static void generateButtonCheckBox(GUIFactory guiFactory) {
		Button b1 = guiFactory.getButton();
		b1.setLabel("Hello!");
		b1.click();
		
		CheckBox ch1 = guiFactory.getCheckBox();
		ch1.setText("Select this if you prefer XYZ ");
		ch1.setStatus(true);
	
		System.out.println(ch1.getStatus());				
	}
	
	public static void main(String[] args) {
		GUIFactory factory = new GUIFactoryWin();
		generateButtonCheckBox(factory);

		System.out.println(" -------------- ------------ ----------  ");
		
		factory = new GUIFactoryLinux();
		generateButtonCheckBox(factory);
	}
}
```