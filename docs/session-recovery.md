# Session recovery checklist

A Kaggle TPU session and its tunnel are temporary. A restarted session may produce a new URL and API key.

1. Open the notebook and inspect its status and latest cell output.
2. If the runtime has stopped, start a new session according to the notebook instructions.
3. Wait until the serving cell confirms readiness and prints fresh connection details.
4. Update local environment variables; never commit the values.
5. Check the endpoint using the command documented by the notebook.
6. Restart the coding agent so it reads the new environment variables.

Do not assume there is a universal 20-minute idle timeout. Session behavior depends on Kaggle's current runtime rules and the implementation. Do not attempt to bypass platform limits with keep-alive tricks.
