# compiler

Compile Java code in memory using Java 6 compiler API.

[![Build Status](https://travis-ci.org/michaelliao/compiler.svg?branch=master)](https://travis-ci.org/michaelliao/compiler)

### Why compiler API?

You can use compiler API to compile generated Java source code and load the compiled classes on-the-fly.

For example, generate proxy classes using compiler API instead of CGLIB or Javassist.

### How to use this compiler?

Step 1: add maven dependency:

```
<dependency>
    <groupId>com.itranswarp</groupId>
    <artifactId>compiler</artifactId>
    <version>1.0</version>
</dependency>
```

Step 2: compile string and load class:

```
public class Main {

    public static void main(String[] args) {
        JavaStringCompiler compiler = new JavaStringCompiler();
        Map<String, byte[]> results = compiler.compile("UserProxy.java", JAVA_SOURCE_CODE);
        Class<?> clazz = compiler.loadClass("on.the.fly.UserProxy", results);
        // try instance:
        User user = (User) clazz.newInstance();
    }

    static final String JAVA_SOURCE_CODE = "/* a single java source file */   "
            + "package on.the.fly;                                            "
            + "public class UserProxy extends test.User {                     "
            + "    boolean _dirty = false;                                    "
            + "    public void setId(String id) {                             "
            + "        super.setId(id);                                       "
            + "        setDirty(true);                                        "
            + "    }                                                          "
            + "    public void setName(String name) {                         "
            + "        super.setName(name);                                   "
            + "        setDirty(true);                                        "
            + "    }                                                          "
            + "    public void setCreated(long created) {                     "
            + "        super.setCreated(created);                             "
            + "        setDirty(true);                                        "
            + "    }                                                          "
            + "    public void setDirty(boolean dirty) {                      "
            + "        this._dirty = dirty;                                   "
            + "    }                                                          "
            + "    public boolean isDirty() {                                 "
            + "        return this._dirty;                                    "
            + "    }                                                          "
            + "}                                                              ";
}
```
