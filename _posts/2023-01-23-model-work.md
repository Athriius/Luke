---
keywords: fastai
description: I have been working on the hacks from last week and figured out how to add classOf to the code.
title: Work with Model OOP
toc: true 
badges: true
comments: true
categories: [jupyter, week-19]
nb_path: _notebooks/2023-01-23-model-work.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2023-01-23-model-work.ipynb
-->

<div class="container" id="notebook-container">
        
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-python"><pre><span></span><span class="kn">import</span> <span class="nn">hashlib</span><span class="o">,</span> <span class="nn">os</span><span class="o">,</span> <span class="nn">binascii</span><span class="o">,</span> <span class="nn">json</span>
<span class="kn">from</span> <span class="nn">datetime</span> <span class="kn">import</span> <span class="n">date</span>

<span class="k">class</span> <span class="nc">User</span><span class="p">:</span>    

    <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">name</span><span class="p">,</span> <span class="n">uid</span><span class="p">,</span> <span class="n">password</span><span class="p">,</span> <span class="n">dob</span><span class="p">,</span> <span class="n">classOf</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_name</span> <span class="o">=</span> <span class="n">name</span>    <span class="c1"># variables with self prefix become part of the object, </span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_uid</span> <span class="o">=</span> <span class="n">uid</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_password</span> <span class="o">=</span> <span class="n">set_password</span><span class="p">(</span><span class="n">password</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_dob</span> <span class="o">=</span> <span class="n">dob</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_classOf</span> <span class="o">=</span> <span class="n">classOf</span> 
    
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">name</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_name</span>
    
    <span class="c1"># a setter function, allows name to be updated after initial object creation</span>
    <span class="nd">@name</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">name</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">name</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_name</span> <span class="o">=</span> <span class="n">name</span>
    
    <span class="c1"># a getter method, extracts email from object</span>
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">uid</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_uid</span>
    
    <span class="c1"># a setter function, allows name to be updated after initial object creation</span>
    <span class="nd">@uid</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">uid</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">uid</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_uid</span> <span class="o">=</span> <span class="n">uid</span>
        
    <span class="c1"># check if uid parameter matches user id in object, return boolean</span>
    <span class="k">def</span> <span class="nf">is_uid</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">uid</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_uid</span> <span class="o">==</span> <span class="n">uid</span>
    
    <span class="c1"># dob property is returned as string, to avoid unfriendly outcomes</span>
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">dob</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="n">dob_string</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">_dob</span><span class="o">.</span><span class="n">strftime</span><span class="p">(</span><span class="s1">&#39;%m-</span><span class="si">%d</span><span class="s1">-%Y&#39;</span><span class="p">)</span>
        <span class="k">return</span> <span class="n">dob_string</span>
    
    <span class="c1"># dob should be have verification for type date</span>
    <span class="nd">@dob</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">dob</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">dob</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_dob</span> <span class="o">=</span> <span class="n">dob</span>
        
    <span class="c1"># age is calculated and returned each time it is accessed</span>
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">age</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="n">today</span> <span class="o">=</span> <span class="n">date</span><span class="o">.</span><span class="n">today</span><span class="p">()</span>
        <span class="k">return</span> <span class="n">today</span><span class="o">.</span><span class="n">year</span> <span class="o">-</span> <span class="bp">self</span><span class="o">.</span><span class="n">_dob</span><span class="o">.</span><span class="n">year</span> <span class="o">-</span> <span class="p">((</span><span class="n">today</span><span class="o">.</span><span class="n">month</span><span class="p">,</span> <span class="n">today</span><span class="o">.</span><span class="n">day</span><span class="p">)</span> <span class="o">&lt;</span> <span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">_dob</span><span class="o">.</span><span class="n">month</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">_dob</span><span class="o">.</span><span class="n">day</span><span class="p">))</span>

    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">classOf</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_classOf</span>
    
    <span class="c1"># a setter function, allows name to be updated after initial object creation</span>
    <span class="nd">@classOf</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">classOf</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">classOf</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_classOf</span> <span class="o">=</span> <span class="n">classOf</span>
    
    <span class="c1"># dictionary is customized, removing password for security purposes</span>
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">dictionary</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="nb">dict</span> <span class="o">=</span> <span class="p">{</span>
            <span class="s2">&quot;name&quot;</span> <span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">name</span><span class="p">,</span>
            <span class="s2">&quot;uid&quot;</span> <span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">uid</span><span class="p">,</span>
            <span class="s2">&quot;dob&quot;</span> <span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">dob</span><span class="p">,</span>
            <span class="s2">&quot;age&quot;</span> <span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">age</span><span class="p">,</span>
            <span class="s2">&quot;classOf&quot;</span> <span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">classOf</span>
        <span class="p">}</span>
        <span class="k">return</span> <span class="nb">dict</span>

         <span class="c1"># output content using json dumps, this is ready for API response</span>
    <span class="k">def</span> <span class="fm">__str__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="n">json</span><span class="o">.</span><span class="n">dumps</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">dictionary</span><span class="p">)</span>
    
    <span class="c1"># output command to recreate the object, uses attribute directly</span>
    <span class="k">def</span> <span class="fm">__repr__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="sa">f</span><span class="s1">&#39;User(name=</span><span class="si">{</span><span class="bp">self</span><span class="o">.</span><span class="n">_name</span><span class="si">}</span><span class="s1">, uid=</span><span class="si">{</span><span class="bp">self</span><span class="o">.</span><span class="n">_uid</span><span class="si">}</span><span class="s1">, password=</span><span class="si">{</span><span class="bp">self</span><span class="o">.</span><span class="n">_password</span><span class="si">}</span><span class="s1">,dob=</span><span class="si">{</span><span class="bp">self</span><span class="o">.</span><span class="n">_dob</span><span class="si">}</span><span class="s1">, classOf=</span><span class="si">{</span><span class="bp">self</span><span class="o">.</span><span class="n">classOf</span><span class="si">}</span><span class="s1">)&#39;</span>
    
<span class="k">def</span> <span class="nf">set_password</span><span class="p">(</span><span class="n">password</span><span class="p">):</span>
    <span class="n">salt</span> <span class="o">=</span> <span class="n">hashlib</span><span class="o">.</span><span class="n">sha256</span><span class="p">(</span><span class="n">os</span><span class="o">.</span><span class="n">urandom</span><span class="p">(</span><span class="mi">60</span><span class="p">))</span><span class="o">.</span><span class="n">hexdigest</span><span class="p">()</span><span class="o">.</span><span class="n">encode</span><span class="p">(</span><span class="s1">&#39;ascii&#39;</span><span class="p">)</span>
    <span class="n">pwdhash</span> <span class="o">=</span> <span class="n">hashlib</span><span class="o">.</span><span class="n">pbkdf2_hmac</span><span class="p">(</span><span class="s1">&#39;sha512&#39;</span><span class="p">,</span> <span class="n">password</span><span class="o">.</span><span class="n">encode</span><span class="p">(</span><span class="s1">&#39;utf-8&#39;</span><span class="p">),</span> <span class="n">salt</span><span class="p">,</span> <span class="mi">1000000</span><span class="p">)</span>
    <span class="n">pwdhash</span> <span class="o">=</span> <span class="n">binascii</span><span class="o">.</span><span class="n">hexlify</span><span class="p">(</span><span class="n">pwdhash</span><span class="p">)</span>
    <span class="k">return</span> <span class="p">(</span><span class="n">salt</span> <span class="o">+</span> <span class="n">pwdhash</span><span class="p">)</span><span class="o">.</span><span class="n">decode</span><span class="p">(</span><span class="s1">&#39;ascii&#39;</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">pw_check</span><span class="p">(</span><span class="n">stored_password</span><span class="p">,</span> <span class="n">provided_password</span><span class="p">):</span>
    <span class="n">salt</span> <span class="o">=</span> <span class="n">stored_password</span><span class="p">[:</span><span class="mi">64</span><span class="p">]</span>
    <span class="n">pwdhash</span> <span class="o">=</span> <span class="n">hashlib</span><span class="o">.</span><span class="n">pbkdf2_hmac</span><span class="p">(</span><span class="s1">&#39;sha512&#39;</span><span class="p">,</span> <span class="n">provided_password</span><span class="o">.</span><span class="n">encode</span><span class="p">(</span><span class="s1">&#39;utf-8&#39;</span><span class="p">),</span> <span class="n">salt</span><span class="o">.</span><span class="n">encode</span><span class="p">(</span><span class="s1">&#39;ascii&#39;</span><span class="p">),</span> <span class="mi">1000000</span><span class="p">)</span>
    <span class="n">stored_password</span> <span class="o">=</span> <span class="n">stored_password</span><span class="p">[</span><span class="mi">64</span><span class="p">:]</span>
    <span class="n">pwdhash</span> <span class="o">=</span> <span class="n">binascii</span><span class="o">.</span><span class="n">hexlify</span><span class="p">(</span><span class="n">pwdhash</span><span class="p">)</span><span class="o">.</span><span class="n">decode</span><span class="p">(</span><span class="s1">&#39;ascii&#39;</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">pwdhash</span> <span class="o">==</span> <span class="n">stored_password</span>
    
   
    

<span class="k">if</span> <span class="vm">__name__</span> <span class="o">==</span> <span class="s2">&quot;__main__&quot;</span><span class="p">:</span>
    <span class="n">u1</span> <span class="o">=</span> <span class="n">User</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;Thomas Edison&#39;</span><span class="p">,</span> <span class="n">uid</span><span class="o">=</span><span class="s1">&#39;toby&#39;</span><span class="p">,</span> <span class="n">password</span><span class="o">=</span><span class="s1">&#39;123toby&#39;</span><span class="p">,</span> <span class="n">dob</span><span class="o">=</span><span class="n">date</span><span class="p">(</span><span class="mi">1847</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="mi">11</span><span class="p">),</span> <span class="n">classOf</span><span class="o">=</span><span class="s1">&#39;2023&#39;</span><span class="p">)</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;JSON ready string:</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span> <span class="n">u1</span><span class="p">,</span> <span class="s2">&quot;</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">)</span> 
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Raw Variables of object:</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span> <span class="nb">vars</span><span class="p">(</span><span class="n">u1</span><span class="p">),</span> <span class="s2">&quot;</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">)</span> 
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Raw Attributes and Methods of object:</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span> <span class="nb">dir</span><span class="p">(</span><span class="n">u1</span><span class="p">),</span> <span class="s2">&quot;</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">)</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Representation to Re-Create the object:</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span> <span class="nb">repr</span><span class="p">(</span><span class="n">u1</span><span class="p">),</span> <span class="s2">&quot;</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

</div>
 

