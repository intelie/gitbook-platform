# Infrastructure Monitoring

Intelie Live generates lots of metrics about all its components periodically (the period depends on the component). Besides those persisted metrics, Live provides by default many Pipes functions to query these and other metrics in realtime.

![Some of the Pipes functions to monitor the environment](<../.gitbook/assets/image (150).png>)

{% hint style="info" %}
Many plugins add specific metrics to Intelie Live using the metrics API.
{% endhint %}

Those metrics can be used in many different ways. The plugin `plugin-templates`, bundled with Intelie Live, provides a simple way to create a System Monitoring dashboard.

![Creating a System Monitoring dashboard](<../.gitbook/assets/image (143).png>)

{% hint style="info" %}
Before creating the dashboard, look for it. Other people could have already created it.
{% endhint %}

This dashboard contains information about different parts of the infrastructure, such as disk, CPU, memory and network.

![Example of the default System Monitoring dashboard](<../.gitbook/assets/image (64).png>)
