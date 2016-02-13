package com.market.privatespaceactivity;

import android.app.ActionBar;
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.AsyncTask;
import android.os.Bundle;
import android.view.MenuItem;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.market.util.Util;
import java.util.HashMap;
import java.util.Map;
import org.json.JSONObject;

public class HaveReadMessageActivity extends Activity
{
  Button haveReadBtn;
  int itemId;
  TextView messageText;
  int userId;

  public void onCreate(Bundle paramBundle)
  {
    super.onCreate(paramBundle);
    setContentView(2130903055);
    getActionBar().setDisplayHomeAsUpEnabled(true);
    this.messageText = ((TextView)findViewById(2131034222));
    this.haveReadBtn = ((Button)findViewById(2131034223));
    this.messageText.setText(getIntent().getStringExtra("message"));
    this.itemId = getIntent().getIntExtra("itemId", -1);
    this.userId = getIntent().getIntExtra("userId", -1);
    this.haveReadBtn.setOnClickListener(new View.OnClickListener()
    {
      public void onClick(View paramAnonymousView)
      {
        new HaveReadMessageActivity.HaveReadTask(HaveReadMessageActivity.this, null).execute(new String[] { "http://10.96.73.76:8080/SecondHandMarket/MessageServlet" });
      }
    });
  }

  public boolean onOptionsItemSelected(MenuItem paramMenuItem)
  {
    switch (paramMenuItem.getItemId())
    {
    default:
    case 16908332:
    }
    while (true)
    {
      return super.onOptionsItemSelected(paramMenuItem);
      finish();
    }
  }

  private class HaveReadTask extends AsyncTask<String, Void, String>
  {
    ProgressDialog progressDialog;

    private HaveReadTask()
    {
    }

    public String doInBackground(String[] paramArrayOfString)
    {
      try
      {
        HashMap localHashMap = new HashMap();
        localHashMap.put("key", "deleteMessage");
        localHashMap.put("userId", String.valueOf(HaveReadMessageActivity.this.userId));
        localHashMap.put("itemId", String.valueOf(HaveReadMessageActivity.this.itemId));
        String str = Util.postRequest(HaveReadMessageActivity.this, paramArrayOfString[0], localHashMap);
        return str;
      }
      catch (Exception localException)
      {
        localException.printStackTrace();
      }
      return null;
    }

    public void onPostExecute(String paramString)
    {
      if (paramString != null)
        try
        {
          String str = new JSONObject(paramString).getString("info");
          this.progressDialog.dismiss();
          if (str.equals("success"))
          {
            HaveReadMessageActivity.this.setResult(-1, null);
            HaveReadMessageActivity.this.finish();
            return;
          }
          if (str.equals("fail"))
          {
            Toast.makeText(HaveReadMessageActivity.this, "操作失败", 0).show();
            return;
          }
        }
        catch (Exception localException)
        {
          localException.printStackTrace();
        }
    }

    public void onPreExecute()
    {
      this.progressDialog = new ProgressDialog(HaveReadMessageActivity.this);
      this.progressDialog.setProgressStyle(0);
      this.progressDialog.setMessage("处理中...");
      this.progressDialog.setCancelable(false);
      this.progressDialog.show();
    }
  }
}