Goal: to get the hex flag from .jar binary programm file.

----
Здравствуй, 47. Вот новое задание для тебя. Наши конкуренты вновь разработали сверхзащищенное приложение, информация из которого нам очень необходима. Мы верим в вас, 47.
----

Flag: 67094ce59c1d670e543114723b96003234cd262a





This is the simple console java app that asks the key but don't want to check it. So we should decompile it to grab the flag.
The jar file is build/obfuscated/BalloonWindCore_JavaSE.jar (don't ask why, I just didn't changed obfuscation settings). This file has been obfuscated by ProGuard 5.3.

We can try to deobfuscate our jar file or simply decompile it using decompiler JD-GUI or something else (Upd: Enigma deobfuscator works better with this jar). We can see the folowing structure:

package com.bunjlabs;

import java.io.PrintStream;
import java.security.MessageDigest;
import java.util.Scanner;

public class LetsObfuscate
{
  public static void main(String[] paramArrayOfString)
  {
    for (paramArrayOfString = 0; paramArrayOfString < 3; paramArrayOfString++)
    {
      System.out.print('.');
      Thread.sleep(500L);
    }
    System.out.println("\nWelcome to my brand new java application!");
    System.out.println("I'll ask you password, you'll give it to me and that's it! No cheating, no confusion, no sense.");
    System.out.println("Please enter the pass:");
    paramArrayOfString = (paramArrayOfString = new Scanner(System.in)).nextLine();
    paramArrayOfString = paramArrayOfString += "1";
    System.out.println("Sorry, I fogot to say - I'm very lasy so I don't wahna check your pass. I don't know you. You are definetly giving me wrong one.");
    if (paramArrayOfString.isEmpty()) {
      System.out.println(a("You win"));
    }
  }

  private static String a(String paramString)
  {
    paramString = (localObject = MessageDigest.getInstance("SHA1")).digest(paramString.getBytes());
    Object localObject = new StringBuffer();
    for (int i = 0; i < paramString.length; i++) {
      ((StringBuffer)localObject).append(Integer.toString((paramString[i] & 0xFF) + 256, 16).substring(1));
    }
    System.out.print("The key is: ");
    return ((StringBuffer)localObject).toString();
  }
}

There are no flag, but we can see it's generating in method private static String a(String paramString), where String paramString is always "You win".
paramArrayOfString variable can't be empty (because of paramArrayOfString = paramArrayOfString += "1"), so paramArrayOfString.isEmpty() is always false.
We need it to be true to see the
We can check out that method private static String a(String paramString) is SHA1 algorithm, and the code is SHA1 from "You win" (without quoters).
Anyway, we can compile our own program if we get reed of obfuscation like

public static void main(String[] paramArrayOfString) throws NoSuchAlgorithmException {
  System.out.println("\nWelcome to my brand new java application!");
  System.out.println("I'll ask you password, you'll give it to me and that's it! No cheating, no confusion, no sense.");
  System.out.println("Please enter the pass:");

  System.out.println(a("You win"));
}

private static String a(String paramString) throws NoSuchAlgorithmException {
    byte[] paramString2 = (MessageDigest.getInstance("SHA1")).digest(paramString.getBytes());
    Object localObject = new StringBuffer();
    for (int i = 0; i < paramString2.length; i++) {
        ((StringBuffer)localObject).append(Integer.toString((paramString2[i] & 0xFF) + 256, 16).substring(1));
    }
    System.out.print("The key is: ");
    return ((StringBuffer)localObject).toString();
}

So, our new program will always give us the flag we wanted.
