# ExemploCallServiceRos

## No Drive do Robis

```
    def reset_pose(self):
        rospy.wait_for_service("/odrive/reset_odometry")
        resetOdom = rospy.ServiceProxy("/odrive/reset_odometry", std_srvs.srv.Trigger)
        resetOdom()
        return Status(StatusCode.OK)

```

## No Gateway do Robis

```
    # 2 = reset pose e 1 = get pose
    def get_config(self, selector, ctx):
        if 2 in selector.fields:
                  return self.driver.reset_pose()
        elif 1 in selector.fields:
            return self.driver.get_pose()
```

## No IS
```
selector = FieldSelector(fields=[2])
request = Message(content=selector, reply_to=subscription)
channel.publish(topic="RobotGateway.0.GetConfig", message=request)
try:
    get_rep = channel.consume(timeout=0.1)    
    print(get_rep.status.code)
except socket.timeout:
    print("timeout reset_ododmetry")
```
