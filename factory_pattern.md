# Factory Method Pattern
## Overview
Factory method is a **creational pattern** that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

## Advantages
- **Avoid coupling** between creator and concrete products.
- Satisfies **single responsiblity principle** as **creation logic is separated with core logic**.
- Satsifies **open closed principle** as **new types of products can be introduced** without breaking existing client code.

## Applicability
- When you don't know beforehand the **exact types and dependencies** of the objects your code should work with. Factory method allows **easy extension of product construction code** independently from rest of code.

```java
interface Button {
	public void setLabel(String label);
    public String getLabel();
    
	public void setBgColour(String colour);
    public String getBgColour();

	public void click();
}

// And multiple concrete classes that implement Button...

public class ButtonFactory {
	public static Button getButton() {
		String platform = System.getProperty("os.name");
		return getButton(platform);
	}

	public static Button getButton(String platform) {
		Button btn = null;

		if (platform.equalsIgnoreCase("Html")) {
			btn = new ButtonHtml();
		} else if (platform.equalsIgnoreCase("Windows 10")) {
			btn = new ButtonWin10();
		} else if (platform.equalsIgnoreCase("MacOs")) {
			btn = new ButtonMacOs();
		} else if (platform.equalsIgnoreCase("Linux")) {
			btn = new ButtonLinux();
		} else {
			new Exception("Unknwon platform type!");
		}

		return btn;
	}
}

```