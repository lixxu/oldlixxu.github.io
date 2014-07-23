---
layout: post
title: "Sublime text安装Package Control"
date: 2014-05-08 11:46:45 +0800
comments: true
categories: Sublime
tags: Sublime
---

<code>Ctrl+\`</code> 或菜单里 `View` -> `Show Console`, 在文本框中输入:

#Sublime3:
<code>
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler())); by = urllib.request.urlopen('http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join(ipp, pf), 'wb').write(by)
</code>

#Sublime2:
<code>
import urllib2,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); by = urllib2.urlopen('http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open(os.path.join(ipp, pf), 'wb').write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
</code>