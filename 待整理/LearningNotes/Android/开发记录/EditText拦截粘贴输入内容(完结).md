## EditText拦截粘贴输入内容

需求：
1.实现输入框最多输入(显示)140个字符。

其实这个需求很好实现，android原生TextView就已经有这个属性了:android:maxLength。但为了演示EditText如何对粘贴输入的内容进行拦截，我们决定自己实现一个android:maxLength属性。

继承EditText实现我们自己的EditText，比如叫CutCopyTextEditText
```java
public class CutCopyTextEditText extends AppCompatEditText {

    private int mSubStringIndex = -1;

    public MXCutCopyTextEditText(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        initAttr(attrs);
    }

    public MXCutCopyTextEditText(Context context, AttributeSet attrs) {
        this(context, attrs, R.attr.editTextStyle);
    }

    public MXCutCopyTextEditText(Context context) {
        this(context, null);
    }

    private void initAttr(AttributeSet attrs){
        final TypedArray ta = getContext().obtainStyledAttributes(attrs, R.styleable.MXCutCopyTextEditText);
        this.mSubStringIndex = ta.getInteger(R.styleable.MXCutCopyTextEditText_MXCutEndIndex, -1);
        ta.recycle();
    }

    public void setTextContent(String textContent){
        handleTextContent(textContent);
    }

    private void handleTextContent(String textContent){
        final int endIndex = this.mSubStringIndex;
        final StringBuilder sb = new StringBuilder(getText().toString());
        sb.append(textContent);
        final int charNumAfterCopy = sb.length();
        if (charNumAfterCopy > endIndex && endIndex > -1) {
            // 多出的字符用空字符代替
            sb.replace(endIndex, charNumAfterCopy, "");
            setText(sb.toString());
            setSelection(sb.length());
        }
    }

    @Override
    public boolean onTextContextMenuItem(int id) {
        // 粘贴
        if (id == android.R.id.paste) {
            final ClipboardManager clipboardManager = (ClipboardManager) getContext().getSystemService(Context.CLIPBOARD_SERVICE);
            if (clipboardManager != null) {
                final ClipData clipData = clipboardManager.getPrimaryClip();
                if (clipData != null) {
                    String textContent = clipData.getItemAt(0).getText().toString();
                    handleTextContent(textContent);
                    return true;
                }
            }
        }
        return super.onTextContextMenuItem(id);
    }

    @Override
    public InputConnection onCreateInputConnection(EditorInfo outAttrs) {
        return new CutSubStringInputConnection(super.onCreateInputConnection(outAttrs), false);
    }

    private class CutSubStringInputConnection extends InputConnectionWrapper{

        /**
         * Initializes a wrapper.
         *
         * <p><b>Caveat:</b> Although the system can accept {@code (InputConnection) null} in some
         * places, you cannot emulate such a behavior by non-null {@link InputConnectionWrapper} that
         * has {@code null} in {@code target}.</p>
         *
         * @param target  the {@link InputConnection} to be proxied.
         * @param mutable set {@code true} to protect this object from being reconfigured to target
         *                another {@link InputConnection}.  Note that this is ignored while the target is {@code null}.
         */
        CutSubStringInputConnection(InputConnection target, boolean mutable) {
            super(target, mutable);
        }

        @Override
        public boolean commitText(CharSequence text, int newCursorPosition) {
            // 系统软键盘输入
            final int endIndex = mSubStringIndex;
            final String textContent = getText().toString();
            final StringBuilder sb = new StringBuilder(textContent);
            sb.append(text);
            final int diffCharNum = endIndex - textContent.length();
            if (diffCharNum > 0 && diffCharNum <= text.length()) {
                return super.commitText(text.subSequence(0, diffCharNum), newCursorPosition);
            }
            if (endIndex > -1 && sb.length() > endIndex) {
                return true;
            }
            return super.commitText(text, newCursorPosition);
        }
    }

    public void setCutEndIndex(int endIndex) {
        this.mSubStringIndex = endIndex;
    }
}

res/values/attr.xml
<declare-styleable name="CutCopyTextEditText">
    <attr name="cutEndIndex" format="integer" />
</declare-styleable>
```

实现类似于android:maxLength属性的功能我们大概需要从三个方面入手：
1.处理setText()时的字符数限制；
2.处理软键盘输入的字符数限制；
3.处理粘贴进EditText中的字符数限制。

对于第一种情况应该很好处理，我们只需要获取到setText()进来的字符数，然后跟属性指定的字符数比较，超过的字符全部替换成空字符；

限制软键盘输入的字符数就要使用到InputConnection了，android中输入法和输入框是通过InputConnection这个接口来交互的，因此这里的解决方案是覆写TextView中的onCreateInputConnection(EditorInfo outAttrs)方法，返回一个自定义的InputConnection对象(InputConnectionWrapper类实现了InputConnection接口)，重写commitText(CharSequence text,int newCursorPosition)方法，该方法在输入法上的字符进入到输入框时会被调用(比如我们在输入法上输入了一串字符，然后我们点击这串字符想把它显示到输入框中，这时就会调用commitText方法)，因此我们可以在该方法中做一些字符串的预处理。

对于粘贴进入输入框中的字符，如果我们能拿到粘贴的事件，所有问题就会迎刃而解，刚好在TextView中有这么一个方法`onTextContextMenuItem(int id)`，在触发系统剪贴板的粘贴，复制，剪切事件时都会回调该方法，粘贴，复制，剪切事件通过id来区分。

总结：
1.通过覆写`onCreateInputConnection(EditorInfo outAttrs)`方法和自定义InputConnectionWrapper可以对输入法输入内容做定制；
2.通过覆写`onTextContextMenuItem(int id)`方法可以实现复制，粘贴，剪切内容的定制；