# Burger Reviewer

Category: REV

Points: 122

Solves: 244

>Try to sneak a pizza into the burger reviewer!

### Solution

```java

import java.util.*;

public class Burgers {
	
	public static boolean bun(String s) {
		return (s.substring(0, 7).equals("LITCTF{") && s.charAt(s.length()-1) == '}');
	}
	
	public static boolean cheese(String s) {
		return (s.charAt(13) == '_' && (int)s.charAt(17) == 95 && s.charAt(19) == '_' && s.charAt(26)+s.charAt(19) == 190 && s.charAt(29) == '_' && s.charAt(34)-5 == 90 && s.charAt(39) == '_');
	}
	
	public static boolean meat(String s) {
		boolean good = true;
		int m = 41;
		char[] meat = {'n', 'w', 'y', 'h', 't', 'f', 'i', 'a', 'i'};
		int[] dif = {4, 2, 2, 2, 1, 2, 1, 3, 3};
		for (int i = 0; i < meat.length; i++) {
			m -= dif[i];
			if (s.charAt(m) != meat[i]) {
				good = false;
				break;
			}
		}
		return good;
	}
	
	public static boolean pizzaSauce(String s) {
		char[] sauce = {'b', 'p', 'u', 'b', 'r', 'n', 'r', 'c'};
		int a = 7; int b = 20; int i = 0; boolean good = true;
		while (a < b) {
			if (s.charAt(a) != sauce[i] || s.charAt(b) != sauce[i+1]) {
				good = false;
				break;
			}
			a++; b--; i += 2;
			while (!Character.isLetter(s.charAt(a))) a++;
			while (!Character.isLetter(s.charAt(b))) b--;
		}
		return good;
	}
	
	public static boolean veggies(String s) {
		int[] veg1 = {10, 12, 15, 22, 23, 25, 32, 36, 38, 40};
		int[] veg = new int[10];
		for (int i = 0; i < veg1.length; i++) {
			veg[i] = Integer.parseInt(s.substring(veg1[i], veg1[i]+1));
		}
		return (veg[0] + veg[1] == 14 && veg[1] * veg[2] == 20 && veg[2]/veg[3]/veg[4] == 1 && veg[3] == veg[4] && veg[3] == 2 && veg[4] - veg[5] == -3 && Math.pow(veg[5], veg[6]) == 125 && veg[7] == 4 && veg[8] % veg[7] == 3 && veg[8] + veg[9] == 9 && veg[veg.length - 1] == 2);
	}

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.println("Can burgers be pizzas? Try making a burger...");
		System.out.print("Enter flag: ");
		String input = in.next();
		in.close();
		
		boolean gotFlag = true;
		
		if (input.length() > 42) {
			System.out.println("This burger iz too big :(");
		} else if (input.length() < 42) {
			System.out.println("This burger iz too small :(");
		} else {
			if (!bun(input)) {
				System.out.println("Wrong bun >:0");
				gotFlag = false;
			}
			
			if (gotFlag) {
				if (!cheese(input)) {
					System.out.println("Hmph. Not good chez :/");
					gotFlag = false;
				}
			}
			
			if (gotFlag) {
				if (!meat(input)) {
					System.out.println("Bah, needs better meat :S");
					gotFlag = false;
				}
			}
			
			if (gotFlag) {
				if (!pizzaSauce(input)) {
					System.out.println("Tsk tsk. You call that pizza sauce? >:|");
					gotFlag = false;
				}
			}
			
			if (gotFlag) {
				if (!veggies(input)) {
					System.out.println("Rotten veggies, ew XP");
					gotFlag = false;
				}
			}
			
			if (gotFlag) {
				System.out.println("Yesyes good burger :D");
			}
		}
	}
}
```

Java rev :sob:. Seems like there are just a few checks we need to get past: `bun`, `cheese`, `meat`, `pizzaSauce`, `veggies`. We also know that the flag length is 42 so we can just do some simple python to reverse the thing:

1. `bun` 
    ```java
    public static boolean bun(String s) {
		return (s.substring(0, 7).equals("LITCTF{") && s.charAt(s.length()-1) == '}');
	}
    ```
    This is the flag wrapper:
    ```py
    flag = ["X"]*42
    flag[:7] = [*"LITCTF{"]
    flag[-1] = "}"
    ```

2. `cheese`
    ```java
    public static boolean cheese(String s) {
		return (s.charAt(13) == '_' && (int)s.charAt(17) == 95 && s.charAt(19) == '_' && s.charAt(26)+s.charAt(19) == 190 && s.charAt(29) == '_' && s.charAt(34)-5 == 90 && s.charAt(39) == '_');
	}
    ```
    These are all the underscores:
    ```py
    cheese = [13, 17, 19, 26, 29, 34, 39]
    for x in cheese: flag[x] = "_"
    ```

3. `meat`
    ```java
    public static boolean meat(String s) {
		boolean good = true;
		int m = 41;
		char[] meat = {'n', 'w', 'y', 'h', 't', 'f', 'i', 'a', 'i'};
		int[] dif = {4, 2, 2, 2, 1, 2, 1, 3, 3};
		for (int i = 0; i < meat.length; i++) {
			m -= dif[i];
			if (s.charAt(m) != meat[i]) {
				good = false;
				break;
			}
		}
		return good;
	}
    ```
    Checks certain indices:
    ```py
    m = 41
    meat = ['n', 'w', 'y', 'h', 't', 'f', 'i', 'a', 'i']
    dif = [4, 2, 2, 2, 1, 2, 1, 3, 3]
    for i in range(len(meat)):
        m-=dif[i]
        flag[m] = meat[i]
    ```

4. `pizzaSauce`
    ```java
    public static boolean pizzaSauce(String s) {
		char[] sauce = {'b', 'p', 'u', 'b', 'r', 'n', 'r', 'c'};
		int a = 7; int b = 20; int i = 0; boolean good = true;
		while (a < b) {
			if (s.charAt(a) != sauce[i] || s.charAt(b) != sauce[i+1]) {
				good = false;
				break;
			}
			a++; b--; i += 2;
			while (!Character.isLetter(s.charAt(a))) a++;
			while (!Character.isLetter(s.charAt(b))) b--;
		}
		return good;
	}
    ```
    Checks more indices, however it's dependent on surrounding characters so we need to first process the numbers in veggies so that `isLetter` works:
    ```py
    veggies = [10, 12, 15, 22, 23, 25, 32, 36, 38, 40]
    for x in veggies: flag[x] = '0'
    sauce = ['b', 'p', 'u', 'b', 'r', 'n', 'r', 'c']
    a,b,i= 7,20,0
    while a < b:
        flag[a] = sauce[i]
        flag[b] = sauce[i+1]
        a+=1
        b-=1
        i+=2
        while flag[a] not in string.ascii_letters: a+=1
        while flag[b] not in string.ascii_letters: b-=1
    ```


5. `veggies`
    ```java
    public static boolean veggies(String s) {
		int[] veg1 = {10, 12, 15, 22, 23, 25, 32, 36, 38, 40};
		int[] veg = new int[10];
		for (int i = 0; i < veg1.length; i++) {
			veg[i] = Integer.parseInt(s.substring(veg1[i], veg1[i]+1));
		}
		return (veg[0] + veg[1] == 14 && veg[1] * veg[2] == 20 && veg[2]/veg[3]/veg[4] == 1 && veg[3] == veg[4] && veg[3] == 2 && veg[4] - veg[5] == -3 && Math.pow(veg[5], veg[6]) == 125 && veg[7] == 4 && veg[8] % veg[7] == 3 && veg[8] + veg[9] == 9 && veg[veg.length - 1] == 2);
	}
    ```
    Checks the numerical values in the flag, we can do this by hand:
    ```py
    flag[22] = '2' # veg[3] == 2 
    flag[23] = '2' # veg[3] == veg[4]
    flag[15] = '4' # veg[2]/veg[3]/veg[4] == 1
    flag[12] = '5' # veg[1] * veg[2] == 20
    flag[10] = '9' # veg[0] + veg[1] == 14
    flag[25] = '5' # veg[4] - veg[5] == -3
    flag[32] = '3' # pow(veg[5], veg[6]) == 125
    flag[36] = '4' # veg[7] == 4
    flag[38] = '7' or '3' # veg[8] % veg[7] == 3
    flag[40] = '2' # veg[veg.length-1] == 2
    flag[38] = '7' # veg[8] + veg[9] = 9
    ```

And we get the flag:
```py
print(''.join(flag))
'LITCTF{bur9r5_c4n_b_pi22a5_if_th3y_w4n7_2}'
```

### Flag

```LITCTF{bur9r5_c4n_b_pi22a5_if_th3y_w4n7_2}```



